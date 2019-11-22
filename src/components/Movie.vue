<template>
<div>
  <h1>{{ title }}</h1>
  <h1>{{ release_date }}</h1>
  <h1>{{ overview }}</h1>
  <h1>{{ poster_path }}</h1>
  <h1>{{ status }}</h1>
</div>
  
</template>

<script>
import axios from "axios";
export default {
  name: "movie",
  data() {
    return {
      movie: [],
      title: "",
      release_date: "",
      overview: "",
      poster_path: "",
      status: ""
    };
  },
  methods: {
    getResult(){
      axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.id}?api_key=08555db9f6be8fa06d4c47bc7e2d3335`)
           .then(response => {
              let movie = response.data;
              console.log(response.data);
              this.id = movie.id,
              this.title = movie.title,
              this.poster_path = movie.poster_path,
              this.vote_average = movie.vote_average,
              this.release_date = movie.release_date,
              this.overview = movie.overview,
              this.status = movie.status
          })
          .catch(error => alert("Falha ao consultar os dados na api."));
    },
  },
  mounted() {
    this.$route.params.id;
    this.getResult();
  },
}
</script>