<template>
  <div class="container">
    <section class="search">
      <input
        type="text"
        v-model="query"
        @keyup="getResult"
        placeholder="Busque um filme por nome ou gênero..."
      />
    </section>
    <section class="card" v-for="result in results" :key="result.id">
      <figure class="card__image">
        <img v-bind:src="'http://image.tmdb.org/t/p/w500/' + result.poster_path" />
        <!-- <img src="./../assets/no-image.png" alt="" v-else /> -->
      </figure>
      <div class="card__container">
        <header class="card__header">
          <div class="card__title">
            <h1>
              <a href="#" @click.prevent="detailsMovie(result.id)">{{ result.title }}</a>
            </h1>
          </div>
          <div class="card__info">
            <div class="card__vote-average">{{ result.vote_average * 10 }}%</div>
            <div class="card__date">{{ result.release_date }}</div>
          </div>
        </header>
        <div class="card__main">
          <div class="card__overview">{{ result.overview }}</div>
          <div class="card__genres">
            <ul>
              <li v-for="genre in result.genres" :key="genre.id">{{ genre.name }}</li>
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
      results: []
    };
  },
  methods: {
    getResult() {
      if (this.query.length >= 6) {
        //OBTEM A LISTA DE FILMES APARTIR DA LISTA
        axios
          .get(
            `https://api.themoviedb.org/3/search/movie?api_key=08555db9f6be8fa06d4c47bc7e2d3335&query=${this.query}&language=pt-BR`
          )
          .then(response => {
            //COLOCA O RESULTADO EM UM TEMPORARIO
            let temp = response.data.results;
            //ZERA A VARIAVEL RESULTS PARA GARANTIR
            this.results = [];

            //FAZ UM LACO EM CADA FILME DA LISTA PEGANDO SEUS DETALHES
            for (let i = 0; i < temp.length; i++) {
              axios
                .get(
                  `https://api.themoviedb.org/3/movie/${temp[i].id}?api_key=08555db9f6be8fa06d4c47bc7e2d3335&language=pt-BR`
                )
                .then(response2 => {
                  //CRIA UM NOVO OBJETO CONTENDO O TITULO O POSTER E OS GENEROS
                  this.results.push({
                    id: temp[i].id,
                    title: temp[i].title,
                    poster_path: temp[i].poster_path,
                    vote_average: temp[i].vote_average,
                    release_date: temp[i].release_date
                      .split("-")
                      .reverse()
                      .join("/"),
                    overview: temp[i].overview,
                    genres: response2.data.genres
                  });
                })
                .catch(error => alert("Falha ao consultar os dados na api."));
            }
          })
          .catch(error => alert("Falha ao consultar os dados na api."));
      } else if (this.query.length == 0) {
        this.results = "";
      }
    },
    detailsMovie(id) {
      this.$router.push({ path: `/movie/${id}` });
    }
  }
};
</script>
