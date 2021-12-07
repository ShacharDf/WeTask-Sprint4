<template>
  <div id="app">
    <app-header />
    <router-view />
  </div>
</template>

<script>
import appHeader from "./cmps/app-header";

export default {
  beforeCreate() {
    this.$store.dispatch({ type: "loadBoards" });
  },
  components: {
    appHeader,
  },
};
</script>

