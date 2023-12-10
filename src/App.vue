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
    --primary-color: #6F423C;
    --background-color: #110D0E;
    --text-color: #F1E5E7;
    --secondary-color: #6F423C;
  }
  body {
    margin: 0;
    background: #090506;
  }
  main {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    height: 100vh;
    width: 100vw;
  }
</style>
