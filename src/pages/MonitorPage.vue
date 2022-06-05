<template>
  <q-page class="bg-black">
    <div class="row justify-center items-start q-ma-es">
      <div class="col bg-white">TIMER</div>
      <div class="col bg-green">ALARM</div>
      <div class="col bg-white">ALARMMESSAGE</div>
    </div>
    <div class="row justify-center items-start q-ma-es">
      <div class="col-8 bg-white">column one</div>
      <div class="col-2 bg-green">HR</div>
      <div class="col-2 bg-white">column three</div>
    </div>

    <div class="row absolute-bottom justify-center items-start q-ma-es">
      <div class="q-gutter-xs">
        <q-btn color="grey" label="Button" v-for="n in 12" :key="`xs-${n}`" />
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
      butConnectText: "CONNECT TO MANIKIN",
      butConnectClass: "q-ma-lg bg-red text-white",
    };
  },
  methods: {
    onResize() {
      console.log(this.$q.screen.height);
      this.$root.$emit("resize", {
        width: this.$q.screen.width,
        height: this.$q.screen.height,
      });
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
