<template>
  <div id="app">
    <MovieDatabase />
  </div>
</template>

<script>
import MovieDatabase from "./components/MovieDatabase.vue";

export default {
  name: "App",
  components: {
    MovieDatabase,
  },
};
</script>

<style>
#app {
  text-align: center;
  margin-top: 2rem;
}
</style>
