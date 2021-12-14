<template>
  <div class="hello">
    <Timer :scheduledTasks="scheduledTasks" />
  </div>
</template>

<script>
import Timer from "../TimerItem.vue";

export default {
  name: "Week-3",
  components: {
    Timer,
  },
  // created: function () { },
  // mounted: function () { },
  data() {
    return {
      scheduledTasks: [
        { fn: "walk fast", args: 2 },
        { fn: "run", args: 2 },
        { fn: "rest", args: 2 },
        { fn: "run", args: 3 },
        { fn: "rest", args: 3 },
        { fn: "run", args: 2 },
        { fn: "rest", args: 2 },
        { fn: "run", args: 3 },
        { fn: "rest", args: 3 },
      ],
    };
  },
  methods: {},
  computed: {},
};
</script>
