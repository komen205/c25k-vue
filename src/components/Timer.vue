<template>
  <div>
    <n-button type="primary" @click="startController">Start</n-button>
  </div>
</template>

<script>
import moment from "moment";
import { NButton } from "naive-ui";

export default {
  name: "Timer",
  components: {
    NButton,
    // Component here
  },
  // *----------------------- P r o p s ----------------------------------------------------------
  props: { scheduledTasks: Array },
  // *----------------------- D a t a -----------------------------------------------------------
  data() {
    return {
      moment,
      tasks: this.scheduledTasks,
    };
  },
  // *----------------------- C o m p u t e d ---------------------------------------------------
  computed: {},
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
    startTimer: async function (durationTimerInMinutes) {
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

        //format diff to minutes:seconds
        this.timeRemainingToEnd = moment.utc(diff.asMilliseconds());
      }, interval);
    },
    startController: function () {
      this.startTime = moment();
      const time = setInterval(() => {
        this.timeElapsed = moment().diff(this.startTime);
        if (this.tasks.length === 0) {
          console.log("Fim");
          this.textToSpeech("Good job");
          clearInterval(time);
          return;
        }

        if (this.status === "none") {
          const { fn } = this.tasks.shift();
          fn();
        }
      }, 1000);
    },
  },
  // *----------------------- W a t c h ---------------------------------------------------------
  watch: {},
};
</script>
