<script setup>
import second1 from "./components/second1/second1.vue";
import Second2 from "./components/second2/second2.vue";
</script>

<template>
  <div class="container">
    <second1/>
    <second2/>
  </div>
</template>

<style scoped>
.container {
  background-color: #fafafa;
  height: 100%;
  width: 100%;
  display: flex;
}

.second1 {
  background-color: #e7bcbc;
  flex: 1;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.second2 {
  background-color: #a0e5a4;
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
