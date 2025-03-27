<script setup lang="ts">
// import { RouterLink, RouterView } from 'vue-router'
import { RouterView } from 'vue-router'
// import HelloWorld from './components/HelloWorld.vue'

import { ref } from 'vue';
import DrawerMenu from './components/DrawerMenu.vue';
import MqttService from './components/MqttService.vue';

const drawer = ref(false);

const setting = ref({
  mqtt_url: "ws://broker-cn.emqx.io:8083",
  mqtt_user: "",
  mqtt_password: "",
  mqtt_topic: "wearable/wab-7d12/",
});


</script>

<template>
  <div class="common-layout">
    <el-drawer v-model="drawer" direction="ltr" size="50%">
      <DrawerMenu v-model="setting" />
    </el-drawer>
    <el-container style="height: 100%;">
      <el-header >
        <el-row style="padding-top: 0.5rem; padding-bottom: 0.5rem;">
          <el-col :span="4">
            <div class="grid-content" />
          </el-col>
          <el-col :span="16">
            <div class="grid-content" />
          </el-col>
          <el-col :span="4">
            <div class="grid-content" style="height: 2em" @click="drawer = true"><el-icon style="font-size: 2rem;"><Setting /></el-icon></div>
          </el-col>
        </el-row>
      </el-header>
      <el-main>
        <RouterView />
      </el-main>
      <el-footer></el-footer>
    </el-container>
  </div>
  <MqttService setting="setting"/>
</template>

<style>
#app {
  width: 100vw;
  height: 100vh !important;
  padding: 0px !important;
  margin: 0px !important;
}
</style>


<style scoped lang="scss">
.common-layout {
  width: 100vw;
  height: 100vh;
}

.el-header,
.el-footer {
  background-color: #B3C0D1;
  color: #333;
  text-align: center;
  height: 3rem;
  position: relative;
  // align-items: center;
  // justify-items: center;
  align-items: center;
}
</style>
