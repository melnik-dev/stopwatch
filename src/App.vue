<template>
  <div class="container">
  <StopWatch v-for="n in count" :key="n"/>
    <div class="add__btn" @click="addStopwatch">+</div>
  </div>
</template>

<script setup>
import StopWatch from './components/StopWatch.vue'
import {ref} from "vue";

const count = ref(1)
function addStopwatch() {
  count.value++
}
</script>

<style>
body {
  background: #353638;
}

.container {
  padding: 70px 45px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: 45px;
  align-items: center;
}
@media (min-width: 768px) {
  .container {
    flex-direction: row;
    flex-wrap: wrap;
    max-width: 500px;
    justify-content: space-between;
  }
}
@media (min-width: 1024px) {
  .container {
    max-width: 775px;
    justify-content: flex-start;
  }
}

.add__btn {
  width: 225px;
  height: 120px;
  background: #696969;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 55px;
  font-weight: 700;
  color: #9E9E9E;
  cursor: pointer;
}
</style>
