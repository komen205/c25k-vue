<template>
  <div class="hello">
    <h1>Current state:</h1>
    {{ timeRemainingToEnd }}
    {{ status }}
    <h2>Time elapsed:</h2>
    {{ moment(timeElapsed).format("mm:ss") }}

    <button v-on:click="startController">Start</button>
  </div>
</template>

<script>
import moment from "moment";

export default {
  name: "HelloWorld",
  components: {},
  created: function () {
    this.moment = moment;
  },
  mounted: function () {},
  data() {
    return {
      endTime: 0,
      timeLeft: 0,
      timeRemainingToEnd: 0,
      duration: 25,
      startTime: 0,
      timeElapsed: 0,
      status: "none",
      arr: [
        { fn: this.run, args: 1 },
        { fn: this.rest, args: 2 },
        { fn: this.run, args: 1 },
        { fn: this.rest, args: 2 },
        { fn: this.run, args: 1 },
        { fn: this.rest, args: 2 },
        { fn: this.run, args: 1 },
        { fn: this.rest, args: 2 },
        { fn: this.run, args: 1 },
        { fn: this.rest, args: 2 },
        { fn: this.run, args: 1 },
        { fn: this.rest, args: 2 },
        { fn: this.run, args: 1 },
        { fn: this.rest, args: 2 },
      ],
    };
  },
  methods: {
    textToSpeech(text) {
      const msg = new SpeechSynthesisUtterance(text);
      msg.volume = 1; // From 0 to 1
      msg.rate = 1; // From 0.1 to 10
      msg.pitch = 1; // From 0 to 2
      msg.lang = "en";
      window.speechSynthesis.speak(msg);
    },
    run: function (time) {
      this.textToSpeech("Run");
      this.status = "run";
      this.startTimer(time);
    },
    rest: function (time) {
      this.textToSpeech("Rest");
      this.status = "rest";
      this.startTimer(time);
    },
    startTimer:  async function (durationTimerInMinutes) {
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
        this.timeRemainingToEnd = moment.utc(diff.asMilliseconds()).format("mm:ss");
      }, interval);
    },
    calculateDifference: function (endTime) {
      const date = new Date();
      return endTime.getTime() - date.getTime();
    },
    startController: function () {
      this.startTime = moment();
      const time = setInterval(() => {
        this.timeElapsed = moment().diff(this.startTime);
        if (this.arr.length === 0) {
          console.log("Fim");
          this.textToSpeech("Good job");
          clearInterval(time);
          return;
        }

        if (this.status === "none") {
          const { fn, args } = this.arr.shift();
          fn.apply(this, [args]);
        }
      }, 1000);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
