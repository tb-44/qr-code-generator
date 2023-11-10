<script setup lang="ts">
import QRCodeGenerator from '@/components/QRCodeGenerator.vue';
</script>

<template>
  <div id="app">
    <h2>QR Code Generator</h2>
    <QRCodeGenerator />
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
}
h2 {
  margin: 0;
  padding: 0;
}
</style>