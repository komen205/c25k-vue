<template>
  <div class="hello">
    <Timer :scheduledTasks="scheduledTasks" />
  </div>
</template>

<script>
import Timer from "../TimerItem.vue";

export default {
  name: "Week-2",
  components: {
    Timer,
  },
  // created: function () { },
  // mounted: function () { },
  data() {
    return {
      scheduledTasks: [
        { fn: "run", args: 2 },
        { fn: "rest", args: 2 },
        { fn: "run", args: 2 },
        { fn: "rest", args: 2 },
        { fn: "run", args: 2 },
        { fn: "rest", args: 2 },
        { fn: "run", args: 2 },
        { fn: "rest", args: 2 },
        { fn: "run", args: 2 },
        { fn: "rest", args: 2 },
        { fn: "run", args: 2 },
      ],
    };
  },
  methods: {},
  computed: {},
};
</script>
