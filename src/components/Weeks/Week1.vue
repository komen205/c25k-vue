<template>
  <div class="hello">
    <h1>Current state:</h1>
    {{ timeRemainingToEndFormatted }}
    {{ status }}
    <h2>Time elapsed:</h2>
    {{ timeElapsedFormatted }}

    <Timer :scheduledTasks="scheduledTasks" />
  </div>
</template>

<script>
import moment from "moment";
import Timer from "../Timer.vue";

export default {
  name: "Week1",
  components: {
    Timer,
  },
  // created: function () { },
  // mounted: function () { },
  data() {
    return {
      timeRemainingToEnd: 0,
      startTime: 0,
      timeElapsed: 0,
      status: "none",
      scheduledTasks: [
        { fn: this.run.bind(this, 1) },
        { fn: this.rest.bind(this, 2) },
        { fn: this.run.bind(this, 1) },
        { fn: this.rest.bind(this, 2) },
        { fn: this.run.bind(this, 1) },
        { fn: this.rest.bind(this, 2) },
        { fn: this.run.bind(this, 1) },
        { fn: this.rest.bind(this, 2) },
        { fn: this.run.bind(this, 1) },
        { fn: this.rest.bind(this, 2) },
        { fn: this.run.bind(this, 1) },
        { fn: this.rest.bind(this, 2) },
        { fn: this.run.bind(this, 1) },
        { fn: this.rest.bind(this, 2) },
      ],
      moment,
    };
  },
  methods: {
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
  },
  computed: {
    timeElapsedFormatted: function () {
      return moment.utc(this.timeElapsed).format("mm:ss");
    },
    timeRemainingToEndFormatted: function () {
      return moment.utc(this.timeRemainingToEnd).format("mm:ss");
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
