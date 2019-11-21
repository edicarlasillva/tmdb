<template>
  <div id="app">
    <header class="hero">
      <h1 class="hero__title">Movies</h1>
    </header>
    <div class="container">
      <router-view />
      <search />
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

<style>
body {
  font-family: "Lato", sans-serif;
}
h1 {
  font-family: "Abel", sans-serif;
}
.hero {
  background-color: #116193;
  padding: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.hero .hero__title {
  color: #00e8e4;
  font-size: 2rem;
}
.container {
  flex-grow: 1;
  margin: 0 auto;
  position: relative;
  width: auto;
  align-items: stretch;
  display: flex;
  flex-direction: column;
  max-width: 1152px;
}
</style>
