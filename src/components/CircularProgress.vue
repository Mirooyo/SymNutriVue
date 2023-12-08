<!-- CircularProgress.vue -->
<template>
    <div class="circular-progress">
      <svg :width="size" :height="size" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
        <circle
          cx="50"
          cy="50"
          :r="radius"
          :stroke-width="strokeWidth"
          :stroke-dasharray="circumference"
          :stroke-dashoffset="progressOffset"
        />
      </svg>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted, watch } from 'vue';
  
  const size = 100;
  const radius = 40;
  const strokeWidth = 10;
  const circumference = 2 * Math.PI * radius;
  
  const progress = ref(100);
  const maxProgress = ref(100);
  const progressOffset = ref(0);
  
  onMounted(() => {
    watch([progress, maxProgress], () => {
      updateProgressOffset();
    });
  
    updateProgressOffset();
  });
  
  const updateProgressOffset = () => {
    progressOffset.value = circumference - ((progress.value / maxProgress.value) * circumference);
  };
  </script>
  
  <style scoped>
  .circular-progress {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  svg {
    transform: rotate(-90deg);
  }
  
  circle {
    fill: transparent;
    stroke: var(--primary-color);
  }
  </style>
  