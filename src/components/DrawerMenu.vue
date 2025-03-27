<template>
  <div>
    <h1>Settings</h1>
  </div>
  <el-divider />
  <div>
    <el-form :model="form" label-width="5rem">
      <el-form-item label="Mqtt Url">
        <el-input v-model="form.mqtt_url" placeholder="Topic"></el-input>
      </el-form-item>
      <el-form-item label="User">
        <el-input v-model="form.mqtt_user" placeholder="Topic"></el-input>
      </el-form-item>
      <el-form-item label="Password">
        <el-input v-model="form.mqtt_password" placeholder="Topic" type="password"></el-input>
      </el-form-item>
      <el-form-item label="Topic">
        <el-input v-model="form.mqtt_topic" placeholder="Topic"></el-input>
      </el-form-item>
    </el-form>
  </div>

</template>

<script lang="ts" setup>
const form = defineModel<{
  mqtt_url: string
  mqtt_user: string
  mqtt_password: string
  mqtt_topic: string
}>({ required: true });
</script>
