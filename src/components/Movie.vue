<template>
  <div class="inner-page">
    <div class="inner-page-top">
      <header class="inner-page-top__header">
        <div class="container align-header">
          <h1 class="inner-page-top__header-title">{{ title }}</h1>
          <span class="inner-page-top__header-date">{{ date }}</span>
        </div>
      </header>
      <article class="article">
        <div class="inner-page-top-left container">
          <div class="inner-page-top-left__overview">
            <h2 class="inner-page-top-left__overview-title">Sinopse</h2>
            <p class="inner-page-top-left__overview-text">{{ overview }}</p>
          </div>
          <div class="inner-page-top-left__overview">
            <h2 class="inner-page-top-left__overview-title">Informações</h2>
            <div class="inner-page-top-left__overview-info">
              <div>
                <h2 class="inner-page-top-left__overview-info-title">Situação</h2>
                <p class="inner-page-top-left__overview-text">{{ status }}</p>
              </div>
              <div>
                <h2 class="inner-page-top-left__overview-info-title">Idioma</h2>
                <p class="inner-page-top-left__overview-text">
                  <ul>
                    <li v-for="spoken_language in spoken_languages" :key="spoken_language.id">{{ spoken_language.name }}</li>
                  </ul>
                </p>
              </div>
              <div>
                <h2 class="inner-page-top-left__overview-info-title">Duração</h2>
                <p class="inner-page-top-left__overview-text">{{ runtime }}min</p>
              </div>
              <div>
                <h2 class="inner-page-top-left__overview-info-title">Orçamento</h2>
                <p class="inner-page-top-left__overview-text">{{ estimate }}</p>
              </div>
              <div>
                <h2 class="inner-page-top-left__overview-info-title">Receita</h2>
                <p class="inner-page-top-left__overview-text">{{ recipe }}</p>
              </div>
              <div>
                <h2 class="inner-page-top-left__overview-info-title">Lucro</h2>
                <p class="inner-page-top-left__overview-text">{{ profit }}</p>
              </div>
            </div>
          </div>
          <div class="inner-page-top-left__overview genre-averange">
            <ul class="inner-page-top-left__overview-genres">
              <li v-for="genre in genres" :key="genre.id">
                {{ genre.name }}
              </li>
            </ul>
            <div class="vote-average">{{ vote_average * 10 }}%</div>
          </div>
        </div>
        <div class="inner-page-top-right">
          <figure>
            <img class="inner-page-top-right-image" v-bind:src="`http://image.tmdb.org/t/p/w500/${poster_path}`" />
          </figure>
        </div>
      </article>
    </div>
    <div class="inner-page__bottom">
     <iframe :src="`https://www.youtube.com/embed/${key}`" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "movie",
  data() {
    return {
      movie: [],
      trailer: [],
      title: "",
      release_date: "",
      overview: "",
      poster_path: "",
      status: "",
      runtime: "",
      budget: "",
      revenue: "",
      poster_path: "",
      spoken_languages: [],
      genres: [], 
      key: "",
      vote_average: ""
    };
  },
  methods: {
    getResult() {
      axios
        .get(
          `https://api.themoviedb.org/3/movie/${this.$route.params.id}?api_key=08555db9f6be8fa06d4c47bc7e2d3335&language=pt-BR`
        )
        .then(response => {
          let movie = response.data;
          console.log(response.data);
          this.id = movie.id;
          this.title = movie.title;
          this.poster_path = movie.poster_path;
          this.vote_average = movie.vote_average;
          this.release_date = movie.release_date;
          this.overview = movie.overview;
          this.status = movie.status;
          this.runtime = movie.runtime;
          this.budget = movie.budget;
          this.revenue = movie.revenue;
          this.poster_path = movie.poster_path;
          this.spoken_languages = movie.spoken_languages;
          this.genres = movie.genres;
          this.vote_average = movie.vote_average;
        })
        .catch(error => alert("Falha ao consultar os dados na api."));
    },
    getResultTrailer(){
      axios.get(` https://api.themoviedb.org/3/movie/${this.$route.params.id}/videos?api_key=08555db9f6be8fa06d4c47bc7e2d3335`)
      .then(response2 => {
        let trailer = response2.data.results.filter(item => {
          return item.type === "Trailer"
        });
        console.log(trailer);
        this.key = trailer[0].key
      })
      .catch(error => alert("Falha ao consultar os dados na api."));
    }
  },
  computed: {
    profit(){
      return (this.revenue - this.budget).toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });   
    },
    recipe(){
      return this.revenue.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });   
    },
    estimate(){
      return this.budget.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' }); 
    },
    date(){
      return this.release_date.split("-").reverse().join("/");
    }
  },
  mounted() {
    this.$route.params.id;
    this.getResult();
    this.getResultTrailer();
  }
};
</script>