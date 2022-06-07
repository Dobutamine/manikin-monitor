<template>
  <q-card>
    <canvas :id="chartId" style="width: 100%"></canvas>
    <q-resize-observer @resize="onResize" />
  </q-card>
</template>

<script>
import * as PIXI from "pixi.js";
import { boot } from "quasar/wrappers";

export default {
  props: {
    chartId: {
      required: true,
      type: String,
    },
    chartHeight: {
      required: true,
      type: String,
    },
    labelColor: {
      required: true,
      type: String,
    },
    labelName: {
      required: true,
      type: String,
    },
    gridEnabled: {
      required: true,
      type: Boolean,
    },
    gridColor: {
      required: true,
      type: String,
    },
    chartColor: {
      required: true,
      type: String,
    },
    monitorStarted: {
      required: true,
      type: String,
    },
    timeframe: {
      required: true,
      type: Number,
    },
    performance: {
      required: true,
      type: Number,
    },
    signalSource: {
      required: true,
      type: String,
    },
  },
  watch: {
    monitorStarted: function (newValue, oldValue) {
      if (newValue == "true") {
        clearInterval(this.timer);
        this.timer = setInterval(this.drawChart, this.performance);
      } else {
        clearInterval(this.timer);
      }
    },
  },
  data() {
    return {
      canvas: null,
      pixiApp: null,
      grid: null,
      chart1: null,
      chart1X: 0,
      chart1Y: 0,
      prevChart1Y: 0,
      chart2: null,
      chart3: null,
      chartEnabled: true,
      width: 1000,
      labelText: null,
      labelStyle: null,
      ecgData: [0, 2, 4, 3, 0, 40, -7, 0, 5, 8, 5, 0],
      abdWave: [],
      yOffset: 10,
      xOffset: 1,
      timer: null,
      bufferChart: 2,
      clock: 0,
      stepsize: 10,
      heartrate: 125,
      heartrateInterval: 1000,
      heartrateCounter: 0,
      ecgStarted: false,
      ecgCounter: 0,
      ecgSignal: 0,
      abpSignal: 0,
      abpStarted: false,
      abpCounter: 0,
      resprate: 45,
    };
  },
  mounted() {
    this.initialize();
  },
  beforeUnmount() {
    // clean up
    this.$el.removeChild(this.pixiApp.view);
    this.pixiApp.destroy();
    this.pixiApp = null;
  },
  methods: {
    ecgWaveForm() {
      let signal = 0;
      // calculate the ecg signal
      if (this.ecgStarted) {
        signal = this.ecgData[this.ecgCounter];
        this.ecgCounter += 1;
      }

      if (this.ecgCounter >= this.ecgData.length) {
        this.ecgStarted = false;
        this.ecgCounter = 0;
      }

      return signal + Math.random() * 3;
    },
    abpWaveForm() {
      let signal1 = 0;
      let signal2 = 0;
      let noOfSteps = this.heartrateInterval / this.performance;
      // so i have 20 steps with a heartrate interval of 1000 and performance of 50
      // in those 20 steps i have to cover PI
      let abp_step = Math.PI / noOfSteps;

      if (this.abpStarted) {
        signal1 = Math.sin(this.abpCounter) * this.chartHeight * 0.5;
        signal2 = Math.sin(this.abpCounter) * this.chartHeight * 0.35;
        this.abpCounter += abp_step;
        if (this.abpCounter > Math.PI) {
          this.abpCounter = 0;
        }
      }

      if (this.abpCounter > Math.PI / 1.4) {
        return signal2;
      } else {
        return signal1;
      }
    },
    drawChart() {
      this.heartrateInterval = 60000 / this.heartrate;
      this.chart1.lineStyle(2, this.chartColor, 1);
      this.chart2.lineStyle(2, this.chartColor, 1);
      this.chart3.lineStyle(2, this.chartColor, 1);

      if (this.chart1X == 0) {
        this.chart1.clear();
        // if bufferchart 3 is running then we can erase buffer chart 2
        switch (this.bufferChart) {
          case 2:
            this.chart3.clear();
            this.bufferChart = 3;
            break;
          case 3:
            this.chart2.clear();
            this.bufferChart = 2;
            break;
        }
      }
      this.chart1.moveTo(this.chart1X, this.prevChart1Y);
      this.chart2.moveTo(this.chart1X, this.prevChart1Y);
      this.chart3.moveTo(this.chart1X, this.prevChart1Y);

      // draw the graph
      this.chart1X += this.stepsize;

      let ySignal = 0;
      switch (this.signalSource) {
        case "ecg":
          ySignal = this.ecgWaveForm();
          break;
        case "abp":
          ySignal = this.abpWaveForm();
          break;
      }
      this.chart1Y = this.chartHeight - 10 - ySignal;
      this.chart1.lineTo(this.chart1X, this.chart1Y);

      // draw the cursor
      this.chart1.lineStyle(0, "0x000000");
      this.chart1.beginFill("0x000000");
      this.chart1.drawRect(this.chart1X, 0, 10, this.chartHeight);
      this.chart1.endFill();

      // draw the buffers
      if (this.bufferChart == 2) {
        this.chart2.lineTo(this.chart1X, this.chart1Y);
      }
      if (this.bufferChart == 3) {
        this.chart3.lineTo(this.chart1X, this.chart1Y);
      }

      // store the previous X
      this.prevChart1Y = this.chart1Y;

      // reset the x coordinate
      if (this.chart1X > this.width) {
        this.chart1X = 0;
        // reset the clock
        this.clock = 0;
      }

      // calculate the signals
      if (this.heartrateCounter > this.heartrateInterval) {
        this.heartrateCounter = 0;
        this.ecgStarted = true;
        this.ecgCounter = 0;
        this.abpStarted = true;
      }

      // increase the clock
      this.heartrateCounter += this.performance;
    },
    drawGrid() {
      if (this.gridEnabled) {
        if (this.grid) {
          this.grid.clear();
        }
        this.grid.lineStyle(1, this.labelColor, 0.5);
        const step = this.chartHeight / 4;
        for (let i = 0; i < this.chartHeight; i = i + step) {
          this.grid.moveTo(0, this.chartHeight - i);
          for (let x = 0; x < this.width; x = x + 4) {
            this.grid.lineStyle(1, this.gridColor, 0.5);
            this.grid.moveTo(x, this.chartHeight - i);
            this.grid.lineTo(x + 2, this.chartHeight - i);
          }
        }
      } else {
        this.grid.clear();
      }
    },
    updateLabel() {
      this.pixiApp.stage.removeChild(this.labelText);
      this.labelText = new PIXI.Text(this.labelName, this.labelStyle);
      this.labelText.x = 10;
      this.labelText.y = 10;
      this.pixiApp.stage.addChild(this.labelText);
    },
    onResize(newsize) {
      if (this.canvas) {
        // get the current width of the canvas
        this.width = this.canvas.getBoundingClientRect().width;
        // calculate the speed
        this.stepsize =
          this.width / ((this.timeframe * 1000) / this.performance);
        // resize the pixiApp renderer
        if (this.pixiApp) {
          this.pixiApp.renderer.resize(this.width, this.chartHeight);
          this.updateLabel();
          this.drawGrid();
        }
      }
    },
    initialize() {
      // get the reference to the canvas
      this.canvas = document.getElementById(this.chartId);
      // set the detail level of the pixi js
      PIXI.settings.RESOLUTION = 2;
      // define a pixi app with the canvas as view
      this.pixiApp = new PIXI.Application({
        view: this.canvas,
        backgroundAlpha: 1.0,
        autoResize: false,
        antialias: true,
        backgroundColor: 0x000000,
      });
      // add the pixi application to the view
      this.$el.appendChild(this.pixiApp.view);
      this.pixiApp.renderer.view.style.display = "block";
      this.pixiApp.renderer.autoResize = false;
      this.pixiApp.stage.interactive = false;

      // draw the label
      this.labelStyle = new PIXI.TextStyle({
        fontFamily: ["Roboto", "Arial"],
        fontSize: 14,
        fontStyle: "normal",
        fontWeight: "normal",
        fill: this.labelColor, // gradient
        stroke: this.labelColor,
        strokeThickness: 0,
        dropShadow: false,
        dropShadowColor: "#000000",
        dropShadowBlur: 4,
        dropShadowAngle: Math.PI / 6,
        dropShadowDistance: 6,
        wordWrap: true,
        wordWrapWidth: 440,
        lineJoin: "round",
      });

      this.chart2 = new PIXI.Graphics();
      this.pixiApp.stage.addChild(this.chart2);

      this.chart3 = new PIXI.Graphics();
      this.pixiApp.stage.addChild(this.chart3);
      this.chart1 = new PIXI.Graphics();
      this.pixiApp.stage.addChild(this.chart1);

      this.grid = new PIXI.Graphics();
      this.pixiApp.stage.addChild(this.grid);
      this.drawGrid();

      this.labelText = new PIXI.Text(this.labelName, this.labelStyle);
      this.labelText.x = 10;
      this.labelText.y = 10;
      this.pixiApp.stage.addChild(this.labelText);
    },
  },
};
</script>

<style></style>
