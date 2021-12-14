<template>
  <div>
    <h1>C25K App</h1>
    <n-space align="center" justify="center">
      <n-icon
        size="40px"
        v-if="currentTabComponent !== null"
        @click="currentTabComponent = null"
      >
        <arrow-back />
      </n-icon>
      <h2>{{ currentTabComponent }}</h2>
    </n-space>

    <n-space v-if="currentTabComponent === null" justify="center">
      <n-button
        v-for="(week, idx) of weeks"
        :key="idx"
        type="info"
        @click="currentTabComponent = week.component"
      >
        {{ week.name }}
      </n-button>
    </n-space>

    <keep-alive>
      <component v-bind:is="currentTabComponent"></component>
    </keep-alive>
    <footer>
      <n-grid :x-gap="12" :y-gap="8" :cols="1" padding="40px">
        <n-grid-item>
          <div>
            <n-icon size="50">
              <logo-github @click="openGitProject" />
            </n-icon>
          </div>
        </n-grid-item>
      </n-grid>
    </footer>
  </div>
</template>

<script>
import Week1 from "./components/Weeks/Week-1.vue";
import Week2 from "./components/Weeks/Week-2.vue";
import Week3 from "./components/Weeks/Week-3.vue";
import { ArrowBackIosTwotone as ArrowBack } from "@vicons/material";
import { LogoGithub } from "@vicons/ionicons5";

import {
  NButton,
  NSpace,
  NIcon,
  NGrid,
  NGridItem,
  NLayout,
  NLayoutHeader,
} from "naive-ui";

export default {
  name: "App",
  components: {
    Week1,
    Week2,
    Week3,
    NButton,
    NSpace,
    ArrowBack,
    NIcon,
    LogoGithub,
    NGrid,
    NGridItem,
    NLayout,
    NLayoutHeader,
  },
  methods: {
    openGitProject() {
      window.open("https://github.com/komen205/c25k-vue");
    },
  },
  data() {
    return {
      weeks: [
        {
          name: "Week 1",
          component: "Week1",
        },
        {
          name: "Week 2",
          component: "Week2",
        },
        {
          name: "Week 3",
          component: "Week3",
        },
      ],
      currentTabComponent: null,
    };
  },
};
</script>

<style>
.icon-align {
  vertical-align: middle;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
