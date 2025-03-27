<template>
  <el-card>
    <h3 @click="random_motor">
      <el-checkbox v-model="enabled" @change="handle_disable"/>
      {{ title }}&nbsp;
      <el-tag type="primary">{{ form.device_id }}</el-tag>&nbsp;
      <el-tag type="success">{{ form.pin_a }}</el-tag>&nbsp;
      <el-tag type="success">{{ form.pin_b }}</el-tag>&nbsp;
      <el-tag type="info">Now: {{ Math.floor(form.power) }}</el-tag>&nbsp;
    </h3>
    <el-row>
      <el-col :span="4">
        <el-tooltip content="direction of motor">
          <el-switch v-model="form.direction" @change="check_interval" />
        </el-tooltip>
      </el-col>
      <el-col :span="20">
        <el-tooltip content="power percentage">
          <el-slider v-model="form.power" @change="check_interval" :marks="marks" />
        </el-tooltip>
      </el-col>
    </el-row>
    <el-divider></el-divider>
    <el-row>
      <el-col :span="4">
        <el-button :type="random_form.enabled ? 'success' : 'info'" @click="handle_random_switch_change">Random</el-button>
      </el-col>
      <el-col :span="7">
        <el-input-number v-model="random_form.interval" controls-position="right" :min="100" :max="10000" :step="100" @change="set_random_timer" />
      </el-col>
      <el-col :span="7">
        <el-input-number v-model="random_form.inverse_chance" controls-position="right" :min="0" :max="1" :step="0.01"/>
      </el-col>
      <el-col :span="6">
        <el-input-number v-model="random_form.change_rate" controls-position="right" :min="1" :max="100" :step="1"/>
      </el-col>
    </el-row>
  </el-card>
</template>

<script setup lang="ts">
import {defineProps, defineModel, ref } from 'vue';
import emitter from '@/utils/eventbus';

const props = defineProps({
  max_power: { type: Number, required: false, default: 90 },
  min_power: { type: Number, required: false, default: 30 },
  title: { type: String, required: false, default: "Motor" },
})

const enabled = ref(true);

const form = defineModel<{
  device_id: string
  pin_a: number
  pin_b: number
  power: number
  direction: boolean
}>({ required: true });


const random_form = ref(
  {
    enabled: false,
    interval: 5000,
    inverse_chance: 0.2,
    change_rate: 10,
  }
)

const marks_map = new Map<number, string>();
marks_map.set(props.max_power, props.max_power.toString());
marks_map.set(props.min_power, props.min_power.toString());
const marks = Object.fromEntries(marks_map);


let random_timer: NodeJS.Timeout | null = null;

const handle_disable = () => {
  random_form.value.enabled = false;
  if(random_timer) {
      clearInterval(random_timer);
      random_timer = null;
  }
  if(enabled.value){
    form.value.power = 50;
  }else{
    form.value.power = 0;
  }
  check_interval();
}

const handle_random_switch_change = () => {
  random_form.value.enabled = !random_form.value.enabled;
  set_random_timer();
}

const set_random_timer = () => {
  if (random_form.value.enabled) {
    random_timer = setInterval(random_motor, random_form.value.interval);
  }else{
    if(random_timer) {
      clearInterval(random_timer);
      random_timer = null;
    }
  }
}

const random_motor = () => {
  form.value.power = form.value.power + (Math.random() - 0.45) * random_form.value.change_rate * 2;
  if(Math.random() < random_form.value.inverse_chance) {
    form.value.direction = !form.value.direction;
  };
  check_interval();
}

const check_interval = () => {
  if(form.value.power === 0){
    form.value.power = 0;
  } else if(form.value.power > props.max_power) {
    form.value.power = props.max_power;
  } else if (form.value.power < props.min_power) {
    form.value.power = props.min_power;
  }
  send_control_msg();
}

const send_control_msg = () => {
  const pwm = Math.floor(form.value.power * 255 / 100);
  if (form.value.pin_a == form.value.pin_b) {
    const pwm_msg = {
      device_id: form.value.device_id,
      msg: {
        pwm: {
          pin: form.value.pin_a,
          value: pwm,
        }
      }
    }
    emitter.emit('mqtt-send', pwm_msg);
    return;
  }
  const pwm_msg_a = {
    device_id: form.value.device_id,
    msg: {
      pwm: {
        pin: form.value.pin_a,
        value: form.value.direction ? pwm : 0,
      }
    }
  }
  emitter.emit('mqtt-send', pwm_msg_a);
  const pwm_msg_b = {
    device_id: form.value.device_id,
    msg: {
      pwm: {
        pin: form.value.pin_b,
        value: form.value.direction ? 0 : pwm,
      }
    }
  }
  emitter.emit('mqtt-send', pwm_msg_b);
}

</script>

<style scoped></style>
