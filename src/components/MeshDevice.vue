<template>
  <el-collapse-item :title="device_id" :name="device_id">
    <div>context</div>
    <div v-for="sensor in sensors" :key="sensor">
      <el-tag>{{ sensor }}</el-tag>
    </div>
  </el-collapse-item>
</template>

<script lang="ts" setup>
import { defineProps } from 'vue';

defineProps({
  device_id: {
    type: String,
    required: true,
  },
  sensors: {
    type: Array as () => string[],
    required: false,
  },
});

</script>
