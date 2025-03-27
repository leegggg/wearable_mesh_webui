<template>
  <div>
    <DirectPWMPush v-show="false" />
    <MotorControl title="cup" v-model="motor_setting" :max_power="80"/>
    <TheWelcome />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import MotorControl from './MotorControl.vue';
import DirectPWMPush from './DirectPWMPush.vue';
import TheWelcome from './TheWelcome.vue';

const motor_setting = ref({
  device_id: "wab-188b0e91dd64",
  pin_a: 5,
  pin_b: 6,
  power: 50,
  direction: false,
});

</script>
