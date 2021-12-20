<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
  <!-- <HelloWorld msg="Hello Vue 3 in CodeSandbox!" /> -->
  <GoogleMap />
</template>

<script>
import GoogleMap from "./components/Map";
import HelloWorldVue from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    // HelloWorld: HelloWorldVue,
    GoogleMap: GoogleMap,
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
