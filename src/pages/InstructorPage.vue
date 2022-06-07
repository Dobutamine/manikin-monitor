<template>
  <q-page class="bg-black">
    <div class="row justify-center q-ma-es">
      <div class="col">
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="HEARTRATE"
          v-model="hr"
        >
        </q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="SpO2(1)"
          v-model="spo2_pre"
        >
        </q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="SpO2(2)"
          v-model="spo2_post"
        >
        </q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="ABP SYSTOLE"
          v-model="abp_syst"
        >
        </q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="ABP DIASTOLE"
          v-model="abp_diast"
        >
        </q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="RESP RATE"
          v-model="resp_rate"
        >
        </q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="TEMP"
          v-model="temp"
        >
        </q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="ETCO2"
          v-model="etco2"
        >
        </q-input>
        <q-select
          class="text-green"
          v-model="sound"
          dark
          dense
          :options="sound_options"
          label="Sounds"
        />
        <q-select
          class="text-green"
          v-model="airway"
          dark
          dense
          :options="airway_options"
          label="Airway"
        />
        <q-btn
          :class="butConnectClass"
          @click="connectToServer"
          color="primary"
          >{{ butConnectText }}</q-btn
        >
      </div>
      <div class="col-1">
        <q-toggle v-model="hrEnabled" color="green" label="HR"></q-toggle>
        <q-toggle v-model="spo2PreEnabled" color="green" label="HR"></q-toggle>
        <q-toggle v-model="spo2PostEnabled" color="green" label="HR"></q-toggle>
        <q-toggle v-model="abpEnabled" color="green" label="HR"></q-toggle>
        <q-toggle v-model="abpEnabled" color="green" label="HR"></q-toggle>
        <q-toggle v-model="respEnabled" color="green" label="HR"></q-toggle>
        <q-toggle v-model="tempEnabled" color="green" label="HR"></q-toggle>
        <q-toggle v-model="etco2Enabled" color="green" label="HR"></q-toggle>
      </div>
      <div class="col-2">
        <q-toggle
          class="text-green"
          v-model="hrAlarmEnabled"
          dark
          color="green"
          label="HR Alarm"
        ></q-toggle>
        <q-toggle
          class="text-green"
          v-model="spo2PreAlarmEnabled"
          dark
          color="green"
          label="SpO2(pre) Alarm"
        ></q-toggle>
        <q-toggle
          class="text-green"
          v-model="spo2PostAlarmEnabled"
          dark
          color="green"
          label="SpO2(post) Alarm"
        ></q-toggle>
        <q-toggle
          class="text-green"
          v-model="abpAlarmEnabled"
          dark
          color="green"
          label="ABP Alarm"
        ></q-toggle>
        <q-toggle
          class="text-green"
          v-model="abpAlarmEnabled"
          dark
          color="green"
          label="ABP Alarm"
        ></q-toggle>
        <q-toggle
          class="text-green"
          v-model="respAlarmEnabled"
          dark
          color="green"
          label="Resp Alarm"
        ></q-toggle>
        <q-toggle
          class="text-green"
          v-model="tempAlarmEnabled"
          dark
          color="green"
          label="Temp Alarm"
        ></q-toggle>
        <q-toggle
          class="text-green"
          v-model="etco2AlarmEnabled"
          dark
          color="green"
          label="etCO2 Alarm"
        ></q-toggle>
      </div>

      <div class="col">
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="HR UPPER"
          v-model="hrUpper"
        ></q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="SPO2(1) UPPER"
          v-model="spo2PreUpper"
        ></q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="SPO2(2) UPPER"
          v-model="spo2PostUpper"
        ></q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="ABP MEAN UPPER"
          v-model="abpMeanUpper"
        ></q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="ABP MEAN UPPER"
          v-model="abpMeanUpper"
        ></q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="RESP RATE UPPER"
          v-model="respUpper"
        ></q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="TEMP UPPER"
          v-model="tempUpper"
        ></q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="ETCO2 UPPER"
          v-model="etco2Upper"
        ></q-input>
        <q-btn
          class="q-ma-md"
          color="primary"
          @click="updateMonitorConfig"
          label="UPDATE MONITOR CONFIG"
        />
      </div>
      <div class="col">
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="HR LOWER"
          v-model="hrLower"
        ></q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="SPO2(1) LOWER"
          v-model="spo2PreLower"
        ></q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="SPO2(2) LOWER"
          v-model="spo2PostLower"
        ></q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="ABP MEAN LOWER"
          v-model="abpMeanLower"
        ></q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="ABP MEAN LOWER"
          v-model="abpMeanLower"
        ></q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="RESP RATE LOWER"
          v-model="respLower"
        ></q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="TEMP LOWER"
          v-model="tempLower"
        ></q-input>
        <q-input
          type="number"
          dark
          dense
          filled
          label-color="green"
          label="ETCO2 LOWER"
          v-model="etco2Lower"
        ></q-input>
      </div>
      <div class="col-3"></div>
    </div>
    <!-- content -->
    Instructor Page
  </q-page>
</template>

<script>
export default {
  name: "InstructorPage",
  data() {
    return {
      hr: 150,
      hrEnabled: true,
      hrAlarmEnabled: true,
      hrUpper: 200,
      hrLower: 80,
      spo2_pre: 95,
      spo2PreEnabled: true,
      spo2PreAlarmEnabled: true,
      spo2PreUpper: 100,
      spo2PreLower: 92,
      spo2_post: 93,
      spo2PostEnabled: true,
      spo2PostAlarmEnabled: true,
      spo2PostUpper: 100,
      spo2PostLower: 92,
      abp_syst: 60,
      abp_diast: 35,
      abpEnabled: true,
      abpAlarmEnabled: true,
      abpMeanUpper: 80,
      abpMeanLower: 40,
      resp_rate: 34,
      respEnabled: true,
      respAlarmEnabled: true,
      respUpper: 80,
      respLower: 20,
      temp: 36.8,
      tempEnabled: true,
      tempAlarmEnabled: true,
      tempUpper: 38,
      tempLower: 36.5,
      pfi: 0.8,
      etco2: 4.5,
      etco2Enabled: true,
      etco2AlarmEnabled: true,
      etco2Upper: 7,
      etco2Lower: 4.5,
      nibdEnabled: true,
      nibdAlarmEnabled: true,
      configChange: false,
      alarmOverride: false,
      sound: "normal",
      sound_options: ["normal", "cry", "grunt"],
      airway: "auto",
      airway_options: ["auto", "closed", "open"],
      websocket: null,
      setVitalsTimer: null,
      butConnectText: "START",
      butConnectClass: "bg-red text-white",
      configUpdateTimer: null,
    };
  },
  mounted() {},
  beforeUnmount() {},
  methods: {
    updateManikinSound() {
      if (this.websocket) {
        if (this.websocket.readyState == 1) {
          switch (this.sound) {
            case "normal":
              this.websocket.send(
                JSON.stringify({
                  target: "manikin",
                  command: "normal",
                  data: {},
                })
              );
              break;
            case "cry":
              this.websocket.send(
                JSON.stringify({
                  target: "manikin",
                  command: "cry",
                  data: {},
                })
              );
              break;
            case "grunt":
              this.websocket.send(
                JSON.stringify({
                  target: "manikin",
                  command: "grunt",
                  data: {},
                })
              );
              break;
          }
        }
      }
    },
    updateManikinAirway() {
      if (this.websocket) {
        if (this.websocket.readyState == 1) {
          switch (this.airway) {
            case "auto":
              this.websocket.send(
                JSON.stringify({
                  target: "manikin",
                  command: "aw_auto",
                  data: {},
                })
              );
              break;
            case "open":
              this.websocket.send(
                JSON.stringify({
                  target: "manikin",
                  command: "open",
                  data: {},
                })
              );
              break;
            case "closed":
              this.websocket.send(
                JSON.stringify({
                  target: "manikin",
                  command: "aw_close",
                  data: {},
                })
              );
              break;
          }
        }
      }
    },
    updateMonitorConfig() {
      if (this.websocket) {
        if (this.websocket.readyState == 1) {
          this.configChange = true;
          this.configUpdateTimer = setTimeout(() => {
            this.configChange = false;
          }, 5000);
          this.websocket.send(
            JSON.stringify({
              target: "mon_config",
              command: "set",
              data: {
                hrEnabled: this.hrEnabled,
                hrAlarmEnabled: this.hrAlarmEnabled,
                hrUpper: parseInt(this.hrUpper),
                hrLower: parseInt(this.hrLower),
                spo2PreEnabled: this.spo2PreEnabled,
                spo2PreAlarmEnabled: this.spo2PreAlarmEnabled,
                spo2PreUpper: parseInt(this.spo2PreUpper),
                spo2PreLower: parseInt(this.spo2PreLower),
                spo2PostEnabled: this.spo2PostEnabled,
                spo2PostAlarmEnabled: this.spo2PostAlarmEnabled,
                spo2PostUpper: parseInt(this.spo2PostUpper),
                spo2PostLower: parseInt(this.spo2PostLower),
                abpEnabled: this.abpEnabled,
                abpAlarmEnabled: this.abpAlarmEnabled,
                abpMeanUpper: parseInt(this.abpMeanUpper),
                abpMeanLower: parseInt(this.abpMeanLower),
                respEnabled: this.respEnabled,
                respAlarmEnabled: this.respAlarmEnabled,
                respUpper: parseInt(this.respUpper),
                respLower: parseInt(this.respLower),
                etco2Enabled: this.etco2Enabled,
                etco2AlarmEnabled: this.etco2AlarmEnabled,
                etco2Upper: parseFloat(this.etco2Upper),
                etco2Lower: parseFloat(this.etco2Lower),
                tempEnabled: this.tempEnabled,
                tempAlarmEnabled: this.tempAlarmEnabled,
                tempUpper: parseFloat(this.tempUpper),
                tempLower: parseFloat(this.tempLower),
                nibdEnabled: this.nibdEnabled,
                nibdAlarmEnabled: this.nibdAlarmEnabled,
                alarmOverride: this.alarmOverride,
              },
            })
          );
        }
      }
    },
    updateVitals() {
      if (this.websocket) {
        if (this.websocket.readyState == 1) {
          this.websocket.send(
            JSON.stringify({
              target: "vitals",
              command: "set",
              data: {
                hr: parseInt(this.hr),
                resp_rate: parseInt(this.resp_rate),
                spo2_pre: parseInt(this.spo2_pre),
                spo2_post: parseInt(this.spo2_post),
                abp_syst: parseInt(this.abp_syst),
                abp_diast: parseInt(this.abp_diast),
                temp: parseFloat(this.temp),
                pfi: parseFloat(this.pfi),
                etco2: parseFloat(this.etco2),
                configChange: this.configChange,
              },
            })
          );
        }
      }
    },
    processData(data) {
      console.log(data);
    },
    connectToServer() {
      this.websocket = new WebSocket("ws://192.168.144.144:3001");

      this.websocket.onopen = (event) => {
        this.setVitalsTimer = setInterval(this.updateVitals, 1000);
        this.butConnectText = "STOP";
        this.butConnectClass = "bg-green text-white";
      };

      this.websocket.onclose = (event) => {
        if (event.wasClean) {
          clearInterval(this.setVitalsTimer);
        } else {
          // e.g. server process killed or network down
          // event.code is usually 1006 in this case
          alert("Connection to manikin unexpectedly died");
          clearInterval(this.setVitalsTimer);
        }
        this.butConnectText = "START";
        this.butConnectClass = "bg-red text-white";
      };

      this.websocket.onerror = (error) => {
        alert(`[error] ${error.message}`);
        clearInterval(this.setVitalsTimer);
      };

      this.websocket.onmessage = (message) => {
        this.processData(message.data);
      };
    },
  },
};
</script>
