<template>
  <div id="app">
    <Calculator />
  </div>
</template>

<script setup>
import Calculator from "./components/Calculator.vue";
</script>

<style>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: #f7f7f7;
}

#app {
  width: 100%;
  display: flex;
  justify-content: center;
}
</style>
