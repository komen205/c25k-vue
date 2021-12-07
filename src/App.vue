<template>
  <div>
    <keep-alive>
      <component v-bind:is="currentTabComponent"></component>
    </keep-alive>
    <n-button
      v-for="(week, idx) of weeks"
      :key="idx"
      type="info"
      @click="currentTabComponent = week.component"
    >
      {{ week.name }}
    </n-button>
  </div>
</template>

<script>
import Week1 from "./components/Weeks/Week1.vue";
import { NButton } from "naive-ui";

export default {
  name: "App",
  components: {
    Week1,
    NButton,
  },
  data() {
    return {
      weeks: [
        {
          name: "Week 1",
          component: "Week1",
        },
      ],
      currentTabComponent: null,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
