<script setup>
import Meals from "./components/Meals.vue";
</script>

<template>
  <main>
    <Meals/>
  </main>
</template>

<style>
  :root {
    --primary-color: #88C0C2;
    --background-color: #A3D0CA;
  }
  body {
    margin: 0;
  }
  main {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    height: 100vh;
    width: 100vw;
  }
</style>
