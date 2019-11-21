<template>
  <div>
    <section class="search">
      <input
        type="text"
        v-model="query"
        @keyup="getResult"
        placeholder="Busque um filme por nome, ano ou gênero..."
      />
    </section>
    <section class="card" v-for="result in results" :key="result.id">
      <figure class="card__image">
        <img
          v-bind:src="'http://image.tmdb.org/t/p/w500/' + result.poster_path"
        />
      </figure>
      <div class="card__container">
        <header class="card__header">
          <div class="card__title">
            <h1>{{ result.title }}</h1>
          </div>
          <div class="card__info">
            <div class="card__vote-average">
              {{ result.vote_average * 10 }}%
            </div>
            <div class="card__date">{{ result.release_date }}</div>
          </div>
        </header>
        <div class="card__main">
          <div class="card__overview">
            {{ result.overview }}
          </div>
          <div class="card__genres">
            <ul>
              <li v-for="genre in result.genres" :key="genre.id">
                {{ genre.name }}
              </li>
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
            `https://api.themoviedb.org/3/search/movie?api_key=08555db9f6be8fa06d4c47bc7e2d3335&query=${this.query}`
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
                  `https://api.themoviedb.org/3/movie/${temp[i].id}?api_key=08555db9f6be8fa06d4c47bc7e2d3335`
                )
                .then(response2 => {
                  //CRIA UM NOVO OBJETO CONTENDO O TITULO O POSTER E OS GENEROS
                  this.results.push({
                    title: temp[i].title,
                    poster_path: temp[i].poster_path,
                    vote_average: temp[i].vote_average,
                    release_date: temp[i].release_date,
                    overview: temp[i].overview,
                    genres: response2.data.genres
                  });
                })
                .catch(error => alert("falha ao consultar dados na api."));
            }
          })
          .catch(error => alert("falha ao consultar dados na api."));
      } else if (this.query.length == 0) {
        this.results = "";
      }
    }
  }
};
</script>

<style scoped>
.search {
  display: flex;
  justify-content: center;
  margin: 40px 0;
}
.search input {
  background-color: #ebebeb;
  width: 100%;
  padding: 15px 20px;
  border-radius: 20px;
  border: 0;
  outline: 0;
}
/* Placeholder */
::placeholder {
  /* Chrome, Firefox, Opera, Safari 10.1+ */
  color: #116193;
  opacity: 1; /* Firefox */
}

:-ms-input-placeholder {
  /* Internet Explorer 10-11 */
  color: #116193;
}

::-ms-input-placeholder {
  /* Microsoft Edge */
  color: #116193;
}
.card {
  background-color: #ebebeb;
  margin-bottom: 30px;
  display: flex;
}
.card__header {
  display: flex;
  flex-direction: column;
}
.card__info {
  display: flex;
  margin-left: 20px;
}
.card__container {
  width: 100%;
}
.card__image img {
  min-width: 250px;
  width: 250px;
}
.card__title {
  background-color: #116193;
  padding: 20px 20px 5px;
  width: 100%;
}
.card__title h1 {
  font-size: 2rem;
  color: #00e8e4;
  margin-left: 80px;
}
.card__main {
  padding: 10px 30px;
}
.card__genres {
  margin-top: 20px;
}
.card__genres ul {
  display: flex;
}
.card__genres li {
  background-color: #fff;
  padding: 5px 10px;
  border: 1px solid #116193;
  color: #116193;
  border-radius: 20px;
  margin-right: 10px;
}
.card__vote-average {
  background-color: #116193;
  border-radius: 100%;
  width: 60px;
  height: 60px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #00e8e4;
  font-size: 1rem;
  font-weight: 400;
  transform: translateY(-50%);
  /* border: 5px solid #00E8E4; */
  box-shadow: 0px 0px 0px 5px #00e8e4 inset;
  margin-right: 20px;
}
.card__date {
  color: #555555;
  font-weight: 300;
}
.card__overview {
  color: #555555;
}
</style>
