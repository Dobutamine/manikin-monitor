<template>
  <q-page class="bg-black">
    <div class="row justify-center q-ma-es" :style="upperRowHeight">
      <div class="col text-yellow"></div>
      <div class="col text-yellow">ALARM</div>
      <div class="col text-black bg-yellow text-center">ALARMMESSAGE</div>
    </div>
    <div class="row justify-center q-ma-es text-green" :style="rowHeight">
      <div class="col-8">II</div>
      <div class="col-2">
        <div class="row">
          <div class="col" style="font-size: 14px">
            <div>HF</div>
            <div style="font-size: 10px">200</div>
            <div style="font-size: 10px">80</div>
          </div>
          <div class="col-10 text-left q-pa-sm" style="font-size: 52px">
            135
          </div>
        </div>
      </div>
      <div class="col-2 text-purple-12">
        <div class="row">
          <div class="col" style="font-size: 14px">
            <div>Pols</div>
            <div style="font-size: 10px">200</div>
            <div style="font-size: 10px">80</div>
          </div>
          <div class="col-10 text-left q-pa-sm" style="font-size: 36px">
            135
          </div>
        </div>
      </div>
    </div>
    <div class="row justify-center q-ma-es text-purple-12" :style="rowHeight">
      <div class="col-8">Pleth(1)</div>
      <div class="col-2">
        <div class="row">
          <div class="col" style="font-size: 14px">
            <div>SpO2</div>
            <div style="font-size: 10px">100</div>
            <div style="font-size: 10px">92</div>
          </div>
          <div class="col-10 text-left q-pa-sm" style="font-size: 52px">97</div>
        </div>
      </div>
      <div class="col-2">
        <div class="row">
          <div class="col" style="font-size: 14px">
            <div>PFI</div>
            <div style="font-size: 10px"></div>
            <div style="font-size: 10px"></div>
          </div>
          <div class="col-10 text-left q-pa-sm" style="font-size: 36px">
            1.2
          </div>
        </div>
      </div>
    </div>
    <div class="row justify-center q-ma-es text-purple-13" :style="rowHeight">
      <div class="col-8">Pleth(2)</div>
      <div class="col-2">
        <div class="row">
          <div class="col" style="font-size: 14px">
            <div>SpO2(2)</div>
            <div style="font-size: 10px">100</div>
            <div style="font-size: 10px">92</div>
          </div>
          <div class="col-10 text-left q-pa-sm" style="font-size: 52px">95</div>
        </div>
      </div>
      <div class="col-2"></div>
    </div>
    <div class="row justify-center q-ma-es text-pink" :style="rowHeight">
      <div class="col-8">ABP</div>
      <div class="col-3">
        <div class="row">
          <div class="col" style="font-size: 14px">
            <div>ABP</div>
            <div style="font-size: 10px">80</div>
            <div style="font-size: 10px">40</div>
          </div>
          <div class="col-10 text-left q-pt-sm" style="font-size: 52px">
            60/40 (42)
          </div>
        </div>
      </div>
      <div class="col-1"></div>
    </div>
    <div class="row justify-center q-ma-es text-white" :style="rowHeight">
      <div class="col-8">Resp</div>
      <div class="col-2">
        <div class="row">
          <div class="col" style="font-size: 14px">
            <div>RF</div>
            <div style="font-size: 10px">80</div>
            <div style="font-size: 10px">20</div>
          </div>
          <div class="col-10 text-left q-pa-sm" style="font-size: 52px">45</div>
        </div>
      </div>
      <div class="col-2"></div>
    </div>
    <div class="row justify-center q-ma-es text-yellow" :style="rowHeight">
      <div class="col-8">CO2</div>
      <div class="col-2">
        <div class="row">
          <div class="col" style="font-size: 14px">
            <div>etCO2</div>
            <div style="font-size: 10px">7.5</div>
            <div style="font-size: 10px">3.5</div>
          </div>
          <div class="col-10 text-left q-pa-sm" style="font-size: 52px">
            4.5
          </div>
        </div>
      </div>
      <div class="col-2"></div>
    </div>
    <div class="row justify-center q-ma-es text-orange" :style="rowHeight">
      <div class="col-3">
        <div class="row">
          <div class="col" style="font-size: 14px">
            <div>NIBD</div>
            <div style="font-size: 10px">80</div>
            <div style="font-size: 10px">40</div>
          </div>
          <div class="col-10 text-left q-pa-sm" style="font-size: 40px">
            60/40 (42)
          </div>
        </div>
      </div>
      <div class="col-1"></div>
      <div class="col-2"></div>
      <div class="col-2 text-green">
        <div class="row">
          <div class="col" style="font-size: 14px">
            <div>Thuid</div>
            <div style="font-size: 10px">38</div>
            <div style="font-size: 10px">36</div>
          </div>
          <div class="col-10 text-left q-pa-sm" style="font-size: 40px">
            37.1
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
        />
        <q-btn
          color="grey"
          style="height: 60px; width: 80px; font-size: 12px"
          label="Nullen"
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
export default {
  name: "MonitorPage",
  data() {
    return {
      websocket: null,
      getVitalsTimer: null,
      rowHeight: "height': 150px",
      upperRowHeight: "height': 50px",
      butConnectText: "CONNECT TO MANIKIN",
      butConnectClass: "q-ma-lg bg-red text-white",
    };
  },
  methods: {
    onResize() {
      this.$root.$emit("resize", {
        width: this.$q.screen.width,
        height: this.$q.screen.height,
      });
      this.rowHeight = `height: ${this.$q.screen.height / 9}px`;
      this.upperRowHeight = `height: ${this.$q.screen.height / 9 / 3}px`;
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
