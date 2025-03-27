<script setup lang="ts">
import ControlPanel from '@/components/ControlPanel.vue';
import DeviceList from '@/components/DeviceList.vue';
import type { MQTTMessage } from '@/typedef/mqtt';

import emitter from '@/utils/eventbus';

const handleMqttSend = (msg: MQTTMessage) => {
  console.log(msg);
  emitter.emit('mqtt-send', msg);
}

</script>


<template>
  <main style="height: 100%;">
    <!-- <TheWelcome /> -->
    <div class="main-layout">
      <div class="control-panel main-panel">
        <ControlPanel @mqtt-send="handleMqttSend"/>
      </div>
      <div class="device-panel main-panel">
        <DeviceList />
      </div>
    </div>
  </main>
</template>


<style scoped lang="scss">
.main-layout {
  display: flex;
  height: 100%;
}

.main-panel {
  padding: 1rem;
}

@media (orientation: landscape) {
  .main-layout {
    flex-direction: row;
  }
  .main-panel {
    height: 100%;
    overflow: scroll;
  }

  .device-panel {
    width: 30%;
  }

  .control-panel {
    width: 70%;
  }
}

@media (orientation: portrait) {
  .main-layout {
    flex-direction: column;
  }

  .main-panel {
    width: 100%;
    height: fit-content;
  }
}
</style>
