<script setup>
import Calculator from "../components/Calculator.vue";
import Weather from "../components/Weather.vue";
import Navbar from "../components/Navbar.vue";
import Footer from "../components/Footer.vue";
</script>

<template>
  <main >
    <Navbar />
    <Weather />
    <Calculator />
    <Footer />
  </main>
</template>

<style scoped>

main {

  width: 100%;
}

</style>
