<template>
  <q-page class="bg-black flex flex-center column">
    Monitor Page

    <q-btn :class="butConnectClass" @click="toggleConnection">
      {{ butConnectText }}</q-btn
    >
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
