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

  sr.onstart = () => {
    console.log("SR has started");
    isRecording.value = true;
  };

  sr.onend = () => {
    console.log("SR has stopped");
    isRecording.value = false;
  };

  // sr.start();

  // setTimeout(() => {
  //   sr.stop();
  // }, 1000);

  //The Transcript(What you will say is going to print it out)
  sr.onresult = (event) => {
    // console.log(event.results);
    for (let i = 0; i < event.results.length; i++) {
      const result = event.results[i];

      if (result.isFinal) CheckForCommand(result);
      //the isFinal is the respond you are going to say to tell the computer to stop either stop recording
    }

    const trans = Array.from(event.results)
      .map((result) => result[0])
      .map((result) => result.transcript)
      // The join() method creates and returns a new string by concatenating all of the elements in an array (or an array-like object), separated by commas or a specified separator string.
      .join("");

    transcript.value = trans;
  };
});

const CheckForCommand = (result) => {
  const trans = result[0].transcript;
  if (trans.includes("stop recording")) {
    sr.stop();
  } else if (
    trans.includes("what is the time?") || // this is an "or" sign
    trans.includes("what's the time")
  ) {
    sr.stop();
    //this is the code implementation to print out the time formart
    alert(new Date().toLocaleTimeString());
    //Timeout is here
    setTimeout(() => sr.start(), 1000);
  }
};

const ToggleMic = () => {
  if (isRecording.value) {
    sr.stop();
  } else {
    sr.start();
  }
};
</script>

<template>
  <div class="app">
    <button :class="`speak-button`" @click="ToggleMic">Record</button>
    <div class="transcript" v-text="transcript"></div>
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
.transcript {
  position: absolute;
  left: 50%;
  top: 40%;
  transform: translate(-50%, -40%);
}
</style>
