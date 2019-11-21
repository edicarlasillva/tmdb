<template>
  <div class="search">
    <h1>Search</h1>
    <input type="text" v-model="query" @keyup="getResult" />
    <section class="card" v-for="result in results" :key="result.id">
      <figure class="card__image">
        <img
          v-bind:src="'http://image.tmdb.org/t/p/w500/' + result.poster_path"
          width="100px"
        />
      </figure>
      <div class="card__container">
        <header class="card__header">
          <div class="card__title">
            <h1>{{ result.title }}</h1>
          </div>
          <div class="card__vote-average">{{ result.vote_average }}</div>
          <div class="card__date">{{ result.release_date }}</div>
        </header>
        <div class="card__main">
          <div class="card__overview">
            {{ result.overview }}
          </div>
          <div class="card__genres">
            <ul>
              <li v-for="genre in genres" :key="genre.id">{{ genre.name }}</li>
            </ul>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "search",
  data() {
    return {
      query: "",
      results: ""
    };
  },
  methods: {
    getResult() {
      if (this.query.length >= 3) {
        axios
          .get(
            `https://api.themoviedb.org/3/search/movie?api_key=08555db9f6be8fa06d4c47bc7e2d3335&query=${this.query}`
          )
          .then(response => {
            this.results = response.data.results;
          })
          .catch(error => alert("Falha ao consultar os dados na api."));
      } else if (this.query.length == 0) {
        this.results = "";
      }
    }
  }
};
</script>
