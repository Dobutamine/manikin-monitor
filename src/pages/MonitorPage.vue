<template>
  <q-page class="bg-black">
    <div class="row justify-center q-ma-es" :style="upperRowHeight">
      <div class="col text-yellow"></div>
      <div class="col text-yellow">ALARM</div>
      <div :class="alarmBannerClass" style="font-size: 20px">
        {{ alarmBannerMessage }}
      </div>
    </div>
    <div class="row justify-center q-ma-es text-green" :style="rowHeight">
      <div class="col-8">
        <ChannelComponent
          chartId="ECG"
          :chartHeight="chartHeight"
          labelColor="#00ff00"
          labelName="II"
          :gridEnabled="false"
          gridColor="0x00ff00"
          chartColor="0x00ff00"
          :monitorStarted="monitorStarted"
          :timeframe="6"
          :performance="performance"
          signalSource="ecg"
          :vitals="vitals"
          :config="config"
        ></ChannelComponent>
      </div>
      <div class="col-2">
        <div class="row">
          <div v-if="config.hrEnabled" class="col" style="font-size: 14px">
            <div>HF</div>
            <div style="font-size: 10px">{{ config.hrUpper }}</div>
            <div style="font-size: 10px">{{ config.hrLower }}</div>
            <div
              v-if="!config.hrAlarmEnabled"
              class="bg-white text-red text-center"
              style="font-size: 14px"
            >
              X
            </div>
          </div>
          <div
            v-if="config.hrEnabled & !hrBlinker"
            class="col-10 text-left q-pa-xs"
            style="font-size: 68px"
          >
            {{ vitals.heartrate }}
          </div>
        </div>
      </div>
      <div class="col-2 text-purple-12">
        <div class="row">
          <div v-if="config.spo2PreEnabled" class="col" style="font-size: 14px">
            <div>Pols</div>
            <div style="font-size: 10px">{{ config.hrUpper }}</div>
            <div style="font-size: 10px">{{ config.hrLower }}</div>
          </div>
          <div
            v-if="config.spo2PreEnabled"
            class="col-10 text-left q-pa-xs"
            style="font-size: 36px"
          >
            {{ vitals.heartrate }}
          </div>
        </div>
      </div>
    </div>

    <div class="row justify-center q-ma-es text-purple-12" :style="rowHeight">
      <div class="col-8">
        <ChannelComponent
          chartId="SpO2(1)"
          :chartHeight="chartHeight"
          labelColor="0xce04fd"
          labelName="Pleth(1)"
          :gridEnabled="true"
          gridColor="0xce04fd"
          chartColor="0xce04fd"
          :monitorStarted="monitorStarted"
          :timeframe="6"
          :performance="performance"
          signalSource="spo2pre"
          :vitals="vitals"
          :config="config"
        ></ChannelComponent>
      </div>
      <div class="col-2">
        <div class="row">
          <div v-if="config.spo2PreEnabled" class="col" style="font-size: 14px">
            <div>SpO2(1)</div>
            <div style="font-size: 10px">{{ config.spo2PreUpper }}</div>
            <div style="font-size: 10px">{{ config.spo2PreLower }}</div>
            <div
              v-if="!config.spo2PreAlarmEnabled"
              class="bg-white text-red text-center"
              style="font-size: 14px"
            >
              X
            </div>
          </div>
          <div
            v-if="config.spo2PreEnabled & !spo2PreBlinker"
            class="col-10 text-left q-pa-xs"
            style="font-size: 68px"
          >
            {{ vitals.spo2Pre }}
          </div>
        </div>
      </div>
      <div class="col-2">
        <div class="row">
          <div v-if="config.spo2PreEnabled" class="col" style="font-size: 14px">
            <div>PFI</div>
            <div style="font-size: 10px"></div>
            <div style="font-size: 10px"></div>
          </div>
          <div
            v-if="config.spo2PreEnabled"
            class="col-10 text-left q-pa-sm"
            style="font-size: 36px"
          >
            {{ vitals.pfi }}
          </div>
        </div>
      </div>
    </div>
    <div class="row justify-center q-ma-es text-purple-13" :style="rowHeight">
      <div class="col-8">
        <ChannelComponent
          chartId="SpO2(2)"
          :chartHeight="chartHeight"
          labelColor="0xde56fd"
          labelName="Pleth(2)"
          :gridEnabled="true"
          gridColor="0xde56fd"
          chartColor="0xde56fd"
          :monitorStarted="monitorStarted"
          :timeframe="6"
          :performance="performance"
          signalSource="spo2post"
          :vitals="vitals"
          :config="config"
        ></ChannelComponent>
      </div>
      <div class="col-2">
        <div class="row">
          <div
            v-if="config.spo2PostEnabled"
            class="col"
            style="font-size: 14px"
          >
            <div>SpO2(2)</div>
            <div style="font-size: 10px">{{ config.spo2PostUpper }}</div>
            <div style="font-size: 10px">{{ config.spo2PostLower }}</div>
            <div
              v-if="!config.spo2PostAlarmEnabled"
              class="bg-white text-red text-center"
              style="font-size: 14px"
            >
              X
            </div>
          </div>
          <div
            v-if="config.spo2PostEnabled & !spo2PostBlinker"
            class="col-10 text-left q-pa-xs"
            style="font-size: 68px"
          >
            {{ vitals.spo2Post }}
          </div>
        </div>
      </div>
      <div class="col-2"></div>
    </div>
    <div class="row justify-center q-ma-es text-pink" :style="rowHeight">
      <div class="col-8">
        <ChannelComponent
          chartId="ABP"
          :chartHeight="chartHeight"
          labelColor="#dd0000"
          labelName="ABP"
          :gridEnabled="true"
          gridColor="0xdd0000"
          chartColor="0xff0000"
          :monitorStarted="monitorStarted"
          :timeframe="6"
          :performance="performance"
          signalSource="abp"
          :vitals="vitals"
          :config="config"
        ></ChannelComponent>
      </div>
      <div class="col-4">
        <div class="row">
          <div v-if="config.abpEnabled" class="col" style="font-size: 14px">
            <div>ABP</div>
            <div style="font-size: 10px">{{ config.abpMeanUpper }}</div>
            <div style="font-size: 10px">{{ config.abpMeanLower }}</div>
            <div
              v-if="!config.abpAlarmEnabled"
              class="bg-white text-red text-center"
              style="font-size: 14px"
            >
              X
            </div>
          </div>
          <div
            v-if="config.abpEnabled & !abpBlinker"
            class="col-11 q-pt-xs"
            style="font-size: 52px"
          >
            {{ vitals.abpSyst }}/{{ vitals.abpDiast }} ({{ vitals.abpMean }})
          </div>
        </div>
      </div>
    </div>
    <div class="row justify-center q-ma-es text-white" :style="rowHeight">
      <div class="col-8">
        <ChannelComponent
          chartId="RF"
          :chartHeight="chartHeight"
          labelColor="0xffffff"
          labelName="RF"
          :gridEnabled="true"
          gridColor="0xffffff"
          chartColor="0xffffff"
          :monitorStarted="monitorStarted"
          :timeframe="20"
          :performance="performance"
          signalSource="resp"
          :vitals="vitals"
          :config="config"
        ></ChannelComponent>
      </div>
      <div class="col-2">
        <div class="row">
          <div v-if="config.respEnabled" class="col" style="font-size: 14px">
            <div>RF</div>
            <div style="font-size: 10px">{{ config.respUpper }}</div>
            <div style="font-size: 10px">{{ config.respLower }}</div>
            <div
              v-if="!config.respAlarmEnabled"
              class="bg-white text-red text-center"
              style="font-size: 14px"
            >
              X
            </div>
          </div>
          <div
            v-if="config.respEnabled & !respBlinker"
            class="col-10 text-left q-pa-sm"
            style="font-size: 68px"
          >
            {{ vitals.resprate }}
          </div>
        </div>
      </div>
      <div class="col-2"></div>
    </div>
    <div class="row justify-center q-ma-es text-yellow" :style="rowHeight">
      <div class="col-8">
        <ChannelComponent
          chartId="etCO2"
          :chartHeight="chartHeight"
          labelColor="0xf9ea03"
          labelName="CO2"
          :gridEnabled="true"
          gridColor="0xf9ea03"
          chartColor="0xf9ea03"
          :monitorStarted="monitorStarted"
          :timeframe="20"
          :performance="performance"
          signalSource="co2"
          :vitals="vitals"
          :config="config"
        ></ChannelComponent>
      </div>

      <div class="col-2">
        <div class="row">
          <div v-if="config.etco2Enabled" class="col" style="font-size: 14px">
            <div>etCO2</div>
            <div style="font-size: 10px">{{ config.etco2Upper }}</div>
            <div style="font-size: 10px">{{ config.etco2Lower }}</div>
            <div
              v-if="!config.etco2AlarmEnabled"
              class="bg-white text-red text-center"
              style="font-size: 14px"
            >
              X
            </div>
          </div>
          <div
            v-if="config.etco2Enabled & !etco2Blinker"
            class="col-10 text-left q-pa-xs"
            style="font-size: 68px"
          >
            {{ vitals.etco2 }}
          </div>
        </div>
      </div>
      <div class="col-2"></div>
    </div>
    <div class="row justify-center q-ma-es text-orange" :style="rowHeight">
      <div class="col-3">
        <div class="row">
          <div v-if="config.nibdEnabled" class="col" style="font-size: 14px">
            <div>NIBD</div>
            <div style="font-size: 10px">{{ config.abpMeanUpper }}</div>
            <div style="font-size: 10px">{{ config.abpMeanLower }}</div>
            <div
              v-if="!config.nibdAlarmEnabled"
              class="bg-white text-red text-center"
              style="font-size: 14px"
            >
              X
            </div>
          </div>
          <div
            v-if="config.nibdEnabled & !nibdBlinker"
            class="col-10 text-left q-pa-sm"
            style="font-size: 40px"
          >
            {{ vitals.abpSyst }}/{{ vitals.abpDiast }} ({{ vitals.abpMean }})
          </div>
        </div>
      </div>
      <div class="col-1"></div>
      <div class="col-2"></div>
      <div class="col-2 text-green">
        <div class="row">
          <div v-if="config.tempEnabled" class="col" style="font-size: 14px">
            <div>Thuid</div>
            <div style="font-size: 10px">{{ config.tempUpper }}</div>
            <div style="font-size: 10px">{{ config.tempLower }}</div>
            <div
              v-if="!config.tempAlarmEnabled"
              class="bg-white text-red text-center"
              style="font-size: 14px"
            >
              X
            </div>
          </div>
          <div
            v-if="config.tempEnabled & !tempBlinker"
            class="col-10 text-left q-pa-sm"
            style="font-size: 40px"
          >
            {{ vitals.temp }}
          </div>
        </div>
      </div>
      <div class="col-2"></div>
      <div class="col-2"></div>
    </div>

    <div class="row absolute-bottom justify-center items-start q-ma-sm">
      <div class="q-gutter-xs">
        <q-btn
          class="text-black"
          color="yellow"
          style="height: 60px; width: 80px; font-size: 12px"
          label="Stil"
        />
        <q-btn
          color="grey"
          style="height: 60px; width: 80px; font-size: 12px"
          label="Pauzeer alarmen"
        />
        <q-btn
          color="grey-8"
          style="height: 60px; width: 80px; font-size: 12px"
          label="<<"
        />
        <q-btn
          color="grey"
          style="height: 60px; width: 80px; font-size: 12px"
          label="Start/ Stop"
          @click="startMonitor"
        />
        <q-btn
          color="grey"
          style="height: 60px; width: 80px; font-size: 12px"
          label="Nullen"
          @click="changeConfigTest"
        />
        <q-btn
          color="grey"
          style="height: 60px; width: 80px; font-size: 12px"
          label="QRS geluid"
        />
        <q-btn
          color="grey"
          style="height: 60px; width: 80px; font-size: 12px"
          label="Voorval zien"
        /><q-btn
          color="grey"
          style="height: 60px; width: 80px; font-size: 12px"
          label="Monitor Standby"
        />
        <q-btn
          color="grey-8"
          style="height: 60px; width: 80px; font-size: 12px"
          label=">>"
        />
        <q-btn
          color="grey"
          style="height: 60px; width: 80px; font-size: 12px"
          label="Hoofd opzet"
        />
        <q-btn
          color="blue-10"
          style="height: 60px; width: 80px; font-size: 12px"
          label="Hoofd scherm"
        />
      </div>
    </div>

    <q-resize-observer @resize="onResize" />
  </q-page>
</template>

<script>
import ChannelComponent from "components/ChannelComponent.vue";

export default {
  name: "MonitorPage",
  components: {
    ChannelComponent,
  },
  data() {
    return {
      websocket: null,
      getVitalsTimer: null,
      rowHeight: "height': 150px",
      upperRowHeight: "height': 50px",
      chartHeight: "200",
      butConnectText: "CONNECT TO MANIKIN",
      butConnectClass: "q-ma-lg bg-red text-white",
      monitorStarted: "false",
      performance: 30,
      vitals: {
        heartrate: 125,
        spo2Pre: 82,
        spo2Post: 80,
        abpSyst: 60,
        abpDiast: 40,
        abpMean: 42,
        resprate: 45,
        etco2: 4.5,
        temp: 37.5,
        pfi: 1.2,
      },
      config: {
        hrEnabled: true,
        hrAlarmEnabled: false,
        hrUpper: 200,
        hrLower: 80,
        spo2PreEnabled: true,
        spo2PreAlarmEnabled: true,
        spo2PreUpper: 100,
        spo2PreLower: 92,
        spo2PostEnabled: false,
        spo2PostAlarmEnabled: false,
        spo2PostUpper: 100,
        spo2PostLower: 92,
        abpEnabled: false,
        abpAlarmEnabled: false,
        abpMeanUpper: 80,
        abpMeanLower: 40,
        respEnabled: true,
        respAlarmEnabled: false,
        respUpper: 80,
        respLower: 30,
        etco2Enabled: false,
        etco2AlarmEnabled: false,
        etco2Upper: 7.0,
        etco2Lower: 4.0,
        tempEnabled: true,
        tempAlarmEnabled: false,
        tempUpper: 38.0,
        tempLower: 36.5,
        nibdEnabled: true,
        nibdAlarmEnabled: true,
        alarmOverride: false,
      },
      blinkerTimer: null,
      blinkerCounter: 0,
      blinkerInterval: 1000,
      blinkerState: false,
      yellowAlarm: true,
      redAlarm: true,
      alarmBannerClass: "col text-black bg-black text-center",
      alarmBannerMessage: "",
      hrBlinker: false,
      spo2PreBlinker: false,
      spo2PostBlinker: false,
      abpBlinker: false,
      respBlinker: false,
      etco2Blinker: false,
      nibdBlinker: false,
      tempBlinker: false,
      alarmHi: null,
      alarmLo: null,
    };
  },
  mounted() {
    this.alarmHi = new Audio("/sounds/alarm_hi.wav");
    this.alarmHi.preload = "auto";
    this.alarmLo = new Audio("/sounds/alarm_lo.wav");
    this.alarmLo.preload = "auto";
  },
  methods: {
    checkAlarms() {
      let yellowAlarm = false;
      let redAlarm = false;
      let alarmMessage = "";

      this.hrBlinker = false;
      if (this.config.hrAlarmEnabled & this.config.hrEnabled) {
        // check hr
        if (this.vitals.heartrate < this.config.hrLower) {
          yellowAlarm = true;
          this.hrBlinker = this.blinker;
          alarmMessage += " *HR<" + this.config.hrLower;
        }
        if (this.vitals.heartrate > this.config.hrUpper) {
          yellowAlarm = true;
          this.hrBlinker = this.blinker;
          alarmMessage += " *HR>" + this.config.hrUpper;
        }
      }

      // check spo2 pre
      this.spo2PreBlinker = false;
      if (this.config.spo2PreAlarmEnabled & this.config.spo2PreEnabled) {
        if (this.vitals.spo2Pre < this.config.spo2PreLower) {
          yellowAlarm = true;
          this.spo2PreBlinker = this.blinker;
          alarmMessage += " *SAT<" + this.config.spo2PreLower;
        }
        if (this.vitals.spo2Pre > this.config.spo2PreUpper) {
          yellowAlarm = true;
          this.spo2PreBlinker = this.blinker;
          alarmMessage += " *SAT>" + this.config.spo2PostUpper;
        }
      }

      // check spo2 post
      this.spo2PostBlinker = false;
      if (this.config.spo2PostAlarmEnabled & this.config.spo2PostEnabled) {
        if (this.vitals.spo2Post < this.config.spo2PostLower) {
          yellowAlarm = true;
          this.spo2PreBlinker = this.blinker;
          alarmMessage += " *SAT<" + this.config.spo2PostLower;
        }
        if (this.vitals.spo2Post > this.config.spo2PostUpper) {
          yellowAlarm = true;
          this.spo2PreBlinker = this.blinker;
          alarmMessage += " *SAT>" + this.config.spo2PostUpper;
        }
      }

      // check abp mean
      this.abpBlinker = false;
      if (this.config.abpAlarmEnabled & this.config.abpEnabled) {
        if (this.vitals.abpMean < this.config.abpMeanLower) {
          yellowAlarm = true;
          this.abpBlinker = this.blinker;
          alarmMessage += " *ABP(m)<" + this.config.abpMeanLower;
        }
        if (this.vitals.abpMean > this.config.abpMeanUpper) {
          yellowAlarm = true;
          this.abpBlinker = this.blinker;
          alarmMessage += " *ABP(m)>" + this.config.abpMeanUpper;
        }
      }

      this.respBlinker = false;
      if (this.config.respAlarmEnabled & this.config.respEnabled) {
        // check resprate
        if (this.vitals.resprate < this.config.respLower) {
          yellowAlarm = true;
          this.respBlinker = this.blinker;
          alarmMessage += " *RESP<" + this.config.respLower;
        }
        if (this.vitals.resprate > this.config.respUpper) {
          yellowAlarm = true;
          this.respBlinker = this.blinker;
          alarmMessage += " *RESP>" + this.config.respUpper;
        }
      }

      this.etco2Blinker = false;
      if (this.config.etco2AlarmEnabled & this.config.etco2Enabled) {
        // check etco2
        if (this.vitals.etco2 < this.config.etco2Lower) {
          yellowAlarm = true;
          this.etco2Blinker = this.blinker;
          alarmMessage += " *ETCO2<" + this.config.etco2Lower;
        }
        if (this.vitals.etco2 > this.config.etco2Upper) {
          yellowAlarm = true;
          this.etco2Blinker = this.blinker;
          alarmMessage += " *ETCO2>" + this.config.etco2Upper;
        }
      }

      this.tempBlinker = false;
      if (this.config.tempAlarmEnabled & this.config.tempEnabled) {
        // check temp
        if (this.vitals.temp < this.config.tempLower) {
          yellowAlarm = true;
          this.tempBlinker = this.blinker;
          alarmMessage += " *TEMP<" + this.config.tempLower;
        }
        if (this.vitals.temp > this.config.tempUpper) {
          yellowAlarm = true;
          this.tempBlinker = this.blinker;
          alarmMessage += " *TEMP>" + this.config.tempUpper;
        }
      }

      // check abp mean
      this.nibdBlinker = false;
      if (this.config.nibdAlarmEnabled & this.config.nibdEnabled) {
        if (this.vitals.abpMean < this.config.abpMeanLower) {
          yellowAlarm = true;
          this.nibdBlinker = this.blinker;
          alarmMessage += " *NIBD(m)<" + this.config.abpMeanLower;
        }
        if (this.vitals.abpMean > this.config.abpMeanUpper) {
          yellowAlarm = true;
          this.nibdBlinker = this.blinker;
          alarmMessage += " *NIBD(m)>" + this.config.abpMeanUpper;
        }
      }

      if (yellowAlarm) {
        this.yellowAlarm = true;
      } else {
        this.yellowAlarm = false;
      }

      if (redAlarm) {
        this.redAlarm = true;
      } else {
        this.redAlarm = false;
      }

      if (yellowAlarm) {
        this.alarmBannerClass = "col text-black bg-yellow text-center";
        this.alarmBannerMessage = alarmMessage;
        // play alarm sound
        this.alarmLo.play();
      } else {
        this.alarmBannerClass = "col text-black bg-black text-center";
      }
      this.blinker = !this.blinker;
    },
    changeConfigTest() {
      this.config.abpEnabled = true;
      this.vitals.heartrate = 60;
      this.vitals.spo2Pre = 95;
      this.config.hrAlarmEnabled = true;
    },
    changeVitalsTest() {
      this.vitals.heartrate = 170;
    },
    startMonitor() {
      if (this.monitorStarted == "true") {
        this.monitorStarted = "false";
        clearInterval(this.blinkerTimer);
      } else {
        this.monitorStarted = "true";
        this.blinkerTimer = setInterval(this.checkAlarms, 1000);
        this.alarmLo.play();
      }
    },
    onResize() {
      this.rowHeight = `height: ${this.$q.screen.height / 9}px`;
      this.upperRowHeight = `height: ${this.$q.screen.height / 9 / 3}px`;
      this.chartHeight = (this.$q.screen.height / 9).toString();
    },
    toggleConnection() {
      if (this.websocket) {
        if (this.websocket.readyState !== 1) {
          this.connectToManikin();
        } else {
          this.disconnectFromManikin();
        }
      } else {
        this.connectToManikin();
      }
    },
    disconnectFromManikin() {
      this.websocket.close();
    },
    connectToManikin() {
      this.websocket = new WebSocket("ws://localhost:3001");

      this.websocket.onopen = (event) => {
        this.getVitals();
        this.butConnectText = "DISCONNECT FROM MANIKIN";
        this.butConnectClass = "q-ma-lg bg-green text-white";
      };

      this.websocket.onclose = (event) => {
        if (event.wasClean) {
          clearInterval(this.getVitalsTimer);
        } else {
          // e.g. server process killed or network down
          // event.code is usually 1006 in this case
          alert("Connection to manikin unexpectedly died");
          clearInterval(this.getVitalsTimer);
        }
        this.butConnectText = "CONNECT TO MANIKIN";
        this.butConnectClass = "q-ma-lg bg-red text-white";
      };

      this.websocket.onerror = (error) => {
        alert(`[error] ${error.message}`);
        clearInterval(this.getVitalsTimer);
      };

      this.websocket.onmessage = (message) => {
        this.processData(message.data);
      };
    },
    processData(data) {
      console.log(data);
    },
    getVitals() {
      clearInterval(this.getVitalsTimer);
      this.getVitalsTimer = setInterval(() => {
        if (this.websocket) {
          if (this.websocket.readyState == 1) {
            this.websocket.send(
              JSON.stringify({ target: "vitals", command: "get" })
            );
          }
        }
      }, 1000);
    },
    getMonConfig() {
      if (this.websocket) {
        if (this.websocket.readyState == 1) {
          this.websocket.send(
            JSON.stringify({ target: "mon_config", command: "get" })
          );
        }
      }
    },
    getLabs() {
      if (this.websocket) {
        if (this.websocket.readyState == 1) {
          this.websocket.send(
            JSON.stringify({ target: "labs", command: "get" })
          );
        }
      }
    },
  },
};
</script>
