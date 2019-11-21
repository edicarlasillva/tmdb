<template>
  <div id="app">
    <h1>Teste</h1>
    <router-view />
    <search />
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

<style scoped></style>
