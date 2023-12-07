<script setup>
import Calculator from './components/calculator.vue'


</script>

<template>
  <div class="container">
    <div class="calculator__container">
      <Calculator />
    </div>
  </div>
</template>

<style scoped>
  .calculator__container {
    max-width: 400px;
    width: 100%;
  }
</style>
