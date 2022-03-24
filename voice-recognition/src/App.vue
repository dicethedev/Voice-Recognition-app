<script setup>
import { ref, onMounted } from "vue";
const transcript = ref("");
const isRecording = ref(false);

const Recognition = window.SpeechRecognition || window.webkitSpeechRecognition;
//this code below will execute output  of the speech
const sr = new Recognition();

onMounted(() => {
  //when you done talking on the App.. this sr.continous will still help to continue listening untill
  //you are done taling or you turn it off
  //intertimResults is going to display this result of the speech on the srceen
  sr.continous = true;
  sr.interimResults = true;

  sr.onStart = () => {
    console.log("Speech has started");
    isRecording.value = true;
  };
  sr.start();
});
</script>

<template>
  <div class="app">
    <button :class="`speak-button`">Record</button>
    <div class="transcript"></div>
  </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Rubik:wght@300;400;500;600&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Rubik", sans-serif;
}

body {
  background: #012d4b;
  color: #fff;
}

.speak-button {
  position: absolute;
  left: 50%;
  top: 30%;
  transform: translate(-50%, -30%);
  padding: 10px;
  background-color: aqua;
  color: #fff;
  border: none;
  border-radius: 10px;
  width: 100px;
  height: 50px;
  font-weight: 400;
  box-shadow: 1px 2px 20px #00cffd;
}
</style>
