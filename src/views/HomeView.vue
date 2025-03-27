<script setup lang="ts">
import TheWelcome from '../components/TheWelcome.vue';
import DirectPWMPush from '../components/DirectPWMPush.vue'
import * as mqtt from "mqtt/dist/mqtt.min";
import { reactive, ref } from "vue";

/**
 * this demo uses EMQX Public MQTT Broker (https://www.emqx.com/en/mqtt/public-mqtt5-broker), here are the details:
 *
 * Broker host: broker.emqx.io
 * WebSocket port: 8083
 * WebSocket over TLS/SSL port: 8084
 */
const connection = reactive({
  // ws or wss
  protocol: "ws",
  host: "broker.emqx.io",
  // ws -> 8083; wss -> 8084
  port: 8083,
  clientId: "emqx_vue3_" + Math.random().toString(16).substring(2, 8),
  /**
   * By default, EMQX allows clients to connect without authentication.
   * https://docs.emqx.com/en/enterprise/v4.4/advanced/auth.html#anonymous-login
   */
  username: "emqx_test",
  password: "emqx_test",
  clean: true,
  connectTimeout: 30 * 1000, // ms
  reconnectPeriod: 4000, // ms
  // for more options and details, please refer to https://github.com/mqttjs/MQTT.js#mqttclientstreambuilder-options
});

const client = ref({
  connected: false,
} as mqtt.MqttClient);
const receivedMessages = ref("");
const subscribedSuccess = ref(false);
const btnLoadingType = ref("");
const retryTimes = ref(0);

const initData = () => {
  client.value = {
    connected: false,
  } as mqtt.MqttClient;
  retryTimes.value = 0;
  btnLoadingType.value = "";
  subscribedSuccess.value = false;
};

const handleOnReConnect = () => {
  retryTimes.value += 1;
  if (retryTimes.value > 5) {
    try {
      client.value.end();
      initData();
      console.log("connection maxReconnectTimes limit, stop retry");
    } catch (error) {
      console.log("handleOnReConnect catch error:", error);
    }
  }
};

// create MQTT connection
const createConnection = () => {
  try {
    btnLoadingType.value = "connect";
    const { protocol, host, port, ...options } = connection;
    const connectUrl = `${protocol}://${host}:${port}/mqtt`;

    /**
     * if protocol is "ws", connectUrl = "ws://broker.emqx.io:8083/mqtt"
     * if protocol is "wss", connectUrl = "wss://broker.emqx.io:8084/mqtt"
     *
     * /mqtt: MQTT-WebSocket uniformly uses /path as the connection path,
     * which should be specified when connecting, and the path used on EMQX is /mqtt.
     *
     * for more details about "mqtt.connect" method & options,
     * please refer to https://github.com/mqttjs/MQTT.js#mqttconnecturl-options
     */
    client.value = mqtt.connect(connectUrl, options);

    if (client.value.on) {
      // https://github.com/mqttjs/MQTT.js#event-connect
      client.value.on("connect", () => {
        btnLoadingType.value = "";
        console.log("connection successful");
      });

      // https://github.com/mqttjs/MQTT.js#event-reconnect
      client.value.on("reconnect", handleOnReConnect);

      // https://github.com/mqttjs/MQTT.js#event-error
      client.value.on("error", (error) => {
        console.log("connection error:", error);
      });

      // https://github.com/mqttjs/MQTT.js#event-message
      client.value.on("message", (topic: string, message) => {
        receivedMessages.value = receivedMessages.value.concat(
          message.toString()
        );
        console.log(`received message: ${message} from topic: ${topic}`);
      });
    }
  } catch (error) {
    btnLoadingType.value = "";
    console.log("mqtt.connect error:", error);
  }
};



</script>


<template>
  <main style="height: 100%;">
    <!-- <TheWelcome /> -->
    <el-container style="height: 100%;">
      <el-main>
        <el-button type="primary" @click="createConnection">Connect</el-button>
        <TheWelcome />
      </el-main>
      <el-aside>
        <el-scrollbar>
          <DirectPWMPush />
          <DirectPWMPush />
          <DirectPWMPush />
          <DirectPWMPush />
          <DirectPWMPush />
        </el-scrollbar>
      </el-aside>

    </el-container>


  </main>
</template>
