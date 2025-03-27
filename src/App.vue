<script setup lang="ts">
// import { RouterLink, RouterView } from 'vue-router'
import { RouterView } from 'vue-router'
// import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <div class="common-layout">
    <el-container style="height: 100%;">
      <el-header></el-header>
      <el-main>
        <RouterView />
      </el-main>
      <el-footer></el-footer>
    </el-container>
  </div>
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
}
</style>
