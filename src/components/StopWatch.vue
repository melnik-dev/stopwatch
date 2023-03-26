<template>
<div class="stopwatch">
  <div class="stopwatch__time">
    {{ time }}
  </div>
  <div class="stopwatch__btn-wrapper">
    <button v-if="!isStart" class="stopwatch__btn stopwatch__start" @click="onStart"></button>
    <button v-else class="stopwatch__btn stopwatch__pause" @click="onPause"></button>
    <button class="stopwatch__btn stopwatch__stop" @click="onStop"></button>
  </div>
</div>
</template>

<script setup>
import {ref} from "vue";

const time = ref('0')
let startTime = ref(0)
let timePassed = ref(0)
let currentTime = ref(0)
let startInterval = ref('')
const isStart = ref(false)

function onStart() {
  if (isStart.value) {
    return
  } else {
    startTime.value = new Date().getTime()
    startInterval.value = setInterval(startStopwatch, 1000)
  }
  isStart.value = true
}
function onPause() {
  if(isStart.value && startTime.value) {
    timePassed.value += currentTime.value - startTime.value
    clearInterval(startInterval.value)
  }
  isStart.value = false
}
function onStop() {
  time.value= '0'
  startTime.value = 0
  timePassed.value = 0
  currentTime.value = 0
  isStart.value = false
  clearInterval(startInterval.value)
}


 function getCurrentTimeNoneZone() {
   let time = new Date()
   // let timeZone = time.getTimezoneOffset() * 60 * 1000
   time = time.getTime()
   currentTime.value = time
   // return timeZone <= 0 ? time + timeZone : time - timeZone
   return time
 }
function startStopwatch() {
  let stopwatch = new Date(getCurrentTimeNoneZone() - startTime.value + timePassed.value)

  let hour = stopwatch.getUTCHours() === 0 ? '' : stopwatch.getUTCHours() + ':'
  let minute = stopwatch.getUTCMinutes() === 0 ? '' : (stopwatch.getUTCHours() === 0 || stopwatch.getUTCMinutes() <= 9) ? stopwatch.getUTCMinutes() + ':' : '0' + stopwatch.getUTCMinutes() + ':'
  let second = (stopwatch.getUTCMinutes() === 0 || stopwatch.getUTCSeconds() >= 9) ? stopwatch.getUTCSeconds() : '0' + stopwatch.getUTCSeconds()

  return time.value = `${hour}${minute}${second}`
}
</script>

<style scoped>
.stopwatch {
  width: 225px;
  height: 120px;
  background: #696969;
  display: flex;
  flex-direction: column;


}
.stopwatch__time, .stopwatch__btn-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-grow: 1;
}
.stopwatch__time {
  border-bottom: 1px solid #9E9E9E;
  font-weight: 400;
  font-size: 22px;
  color: #9E9E9E;
}
.stopwatch__btn-wrapper {
  gap: 50px;
}
.stopwatch__btn {
  width: 20px;
  height: 20px;
  position: relative;
  border: none;
  background: transparent;
  cursor: pointer;
}
.stopwatch__start::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  width: 0;
  height: 0;
  border-style: solid;
  border-width: 10px 0 10px 17px;
  border-color: transparent transparent transparent #9E9E9E;
}
.stopwatch__pause::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  width: 3px;
  height: 20px;
  background: #9E9E9E;
  box-shadow: 7px 0 0 0 #9E9E9E;
}
.stopwatch__stop {
  background: #9E9E9E;
}
</style>