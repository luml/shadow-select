<template>
  <ShadowSelect />
</template>

<script>
import ShadowSelect from "./components/ShadowSelect.vue";

export default {
  name: "App",
  components: {
    ShadowSelect,
  },
};
</script>

<style lang="scss">
body {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
