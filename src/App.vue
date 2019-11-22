<template>
  <div id="app">
    <header class="hero">
      <h1 class="hero__title">Movies</h1>
    </header>
    <div class="container">
      <router-view />
    </div>
  </div>
</template>

<script>
import Search from "./components/Search";
import Card from "./components/Card";
export default {
  name: "App",
  components: {
    Search,
    Card
  }
};
</script>
