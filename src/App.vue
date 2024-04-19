<template>
  <img alt="Vue logo" src="./assets/logo.png" class="hidden" />
  <HelloWorld msg="Welcome to Your Vue.js App" class="hidden" />
  <TreeSelect />
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import TreeSelect from "./components/TreeSelect/TreeSelect.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
    TreeSelect,
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
