<template>
  <div>
    <div>
      <h1>Current state:</h1>
      <n-space justify="center">
        <RadialProgress
          :diameter="200"
          :completed-steps="completedSteps"
          :total-steps="totalSteps"
        >
          {{ status }}
        </RadialProgress>
      </n-space>

      <h2>Time elapsed:</h2>
      {{ timeElapsedFormatted }}
    </div>
    <n-space justify="center">
      <n-button type="primary" @click="startController">Start</n-button>
    </n-space>
  </div>
</template>

<script>
import RadialProgress from "vue3-radial-progress";
import moment from "moment";
import { NButton, NSpace } from "naive-ui";

export default {
  name: "TimerItem",
  components: {
    NButton,
    NSpace,
    RadialProgress,

    // Component here
  },
  // *----------------------- P r o p s ----------------------------------------------------------
  props: { scheduledTasks: Array },
  // *----------------------- D a t a -----------------------------------------------------------
  data() {
    return {
      completedSteps: 0,
      totalSteps: 0,
      timeRemainingToEnd: 0,
      startTime: 0,
      timeElapsed: 0,
      status: "none",
      moment,
      tasks: this.scheduledTasks,
    };
  },
  // *----------------------- C o m p u t e d ---------------------------------------------------
  computed: {
    timeElapsedFormatted: function () {
      return moment.utc(this.timeElapsed).format("mm:ss");
    },
    timeRemainingToEndFormatted: function () {
      return moment.utc(this.timeRemainingToEnd).format("mm:ss");
    },
  },
  // *----------------------- L i f e   c i r c l e ---------------------------------------------
  created() {},
  // *----------------------- M e t h o d s -----------------------------------------------------
  methods: {
    textToSpeech(text) {
      const msg = new SpeechSynthesisUtterance(text);
      msg.volume = 1; // From 0 to 1
      msg.rate = 1; // From 0.1 to 10
      msg.pitch = 1; // From 0 to 2
      msg.lang = "en";
      window.speechSynthesis.speak(msg);
    },
    startTimer(durationTimerInMinutes) {
      const interval = 1000;
      const expected = moment();
      expected.add(interval, "milliseconds");
      expected.add(durationTimerInMinutes, "minutes");

      //create acurate timer between two dates
      const timer = setInterval(() => {
        //calculate the difference between now and expected
        const diff = moment.duration(expected.diff(moment()));

        if (diff < 0) {
          clearInterval(timer);
          this.status = "none";
        }
        this.completedSteps += 1;
        //format diff to minutes:seconds
        this.timeRemainingToEnd = moment.utc(diff.asMilliseconds());
      }, interval);
    },
    startController() {
      this.startTime = moment();
      console.log("test");

      const time = setInterval(() => {
        this.timeElapsed = moment().diff(this.startTime);
        if (this.tasks.length === 0) {
          console.log("Fim");
          this.textToSpeech("Good job");
          clearInterval(time);
          return;
        }

        if (this.status === "none") {
          this.completedSteps = 1;
          const { fn, args } = this.tasks.shift();
          this.totalSteps = args * 60;
          this.status = fn;
          this.startTimer(args);
          this.textToSpeech(fn);
        }
      }, 1000);
    },
  },
  // *----------------------- W a t c h ---------------------------------------------------------
  watch: {},
};
</script>
<style scoped></style>
