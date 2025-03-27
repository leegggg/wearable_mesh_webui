<template>
  <el-card>
    <h3>Direct PWM Push</h3>
    <el-form label-width="auto" label-position="left">
      <el-form-item label="pin:">
        <el-input id="pin" v-model="form.pin" type="number" min="0" max="255" />
      </el-form-item>
      <el-form-item label="pwm:">
        <el-input id="pwm" v-model="form.pwm" type="number" min="0" max="255" />
      </el-form-item>
      <el-form-item label="topic:">
        <el-input id="topic" v-model="form.topic" type="text" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="handleSubmit">Create</el-button>
      </el-form-item>
    </el-form>
  </el-card>
</template>

<script lang="ts">
export default {
  name: "DirectPWMPush",
  data() {
    return { form: { pin: "7", pwm: 127, topic: "" } };
  },
  methods: {
    handleSubmit() {
      console.log("Input 1:", this.form.pin);
      console.log("Input 2:", this.form.pwm);
      console.log("Input 3:", this.form.topic);
    },
  },
};
</script>

<style scoped></style>
