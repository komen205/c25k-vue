<template>
  <div class="hello">
    {{ timeLeft }}
    {{ status }}
    <button v-on:click="startController">Start</button>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  components: {},
  data() {
    return {
      endTime: 0,
      timeLeft: 0,
      duration: 25,
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
    startTimer: function (time) {
      const endTime = new Date();
      endTime.setMinutes(endTime.getMinutes() + time);
      const timer = setInterval(() => {
        const distance = this.calculateDifference(endTime);

        if (distance < 0) {
          clearInterval(timer);
          this.status = "none";
          return;
        }

        this.timeLeft = this.msToMMss(distance);
      }, 1000);
    },
    calculateDifference: function (endTime) {
      const date = new Date();
      return endTime.getTime() - date.getTime();
    },
    msToMMss: function (duration) {
      let minutes = Math.floor((duration / (1000 * 60)) % 60),
        seconds = Math.floor((duration / 1000) % 60);

      seconds = seconds < 10 ? "0" + seconds : seconds;
      minutes = minutes < 10 ? "0" + minutes : minutes;

      return `${minutes}  : ${seconds}`;
    },
    startController: function () {
      const time = setInterval(() => {
        if (this.arr.length === 0) {
          console.log("Fim");
          clearInterval(time);
          return;
        }

        if (this.status === "none") {
          const { fn, args } = this.arr.shift();
          fn.apply(this, [args]);
        }
        console.log(this.arr);
      }, 900);
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
