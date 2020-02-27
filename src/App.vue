<template>
  <div id="app">
    <Main />
  </div>
</template>

<script>
import Main from "./components/main.vue";

export default {
  name: "App",
  components: {
    Main
  }
};
</script>

<style lang="scss">
@import "@/design/index.scss";
</style>
