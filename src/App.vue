<template>
  <keep-alive>
    <component v-bind:is="currentTabComponent"></component>
  </keep-alive>

  <n-button type="info" v-on:click="currentTabComponent = 'Week1'">
    Week 1
  </n-button>
</template>

<script>
import Week1 from "./components/Weeks/Week1.vue";
import { NButton } from "naive-ui";

export default {
  name: "App",
  components: {
    Week1: Week1,
    NButton,
  },
  data() {
    return {
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
