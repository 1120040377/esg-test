<script setup>
import { RouterView } from 'vue-router'
</script>

<template>
  <n-modal-provider>
    <RouterView />
  </n-modal-provider>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box
}
</style>
