<template>
  <q-card>
    <canvas :id="chartId" style="width: 100%"></canvas>
    <q-resize-observer @resize="onResize" />
  </q-card>
</template>

<script>
import * as PIXI from "pixi.js";

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
    gridColor: {
      required: true,
      type: String,
    },
  },
  data() {
    return {
      canvas: null,
      pixiApp: null,
      grid: null,
      gridEnabled: true,
      width: 1000,
      labelText: null,
      labelStyle: null,
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

      this.labelText = new PIXI.Text(this.labelName, this.labelStyle);
      this.labelText.x = 10;
      this.labelText.y = 10;
      this.pixiApp.stage.addChild(this.labelText);

      this.grid = new PIXI.Graphics();
      this.pixiApp.stage.addChild(this.grid);
      this.drawGrid();
    },
  },
};
</script>

<style></style>
