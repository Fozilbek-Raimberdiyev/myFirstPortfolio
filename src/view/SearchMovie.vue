<template>
  <div class="wrapper">
    <div class="search-form" :class="{open : isOpen}">
      <input @keyup="getSearchedMoviesInput" type="search" placeholder="Search" v-model="search" class="search-input">
      <!-- <button @keyup="getSearchedMovies()" type="buttton">Search</button> -->
      
    </div>
    <h1 class="title" v-if="isEnd">Qidiruv natijalari</h1>
    <div class="info-container" v-for="(result,index) in results" :key="index">
    <div class="movie-info" v-if="results.length">
      <div class="movie-img">
        <img :src="`https://image.tmdb.org/t/p/w500//${result.backdrop_path}`" alt="movie-img">
      </div>
      <div class="movie-data">
        <p class="movie-title">{{result.title}}</p>
        <p class="movie-release-year">Release date: <span>{{result.release_date}}</span></p>
        <p class="movie-genre">Genre: 
        <span v-for="(genre,index) in result.genres" :key="index">{{genre.name}}</span>
        </p>
        <p class="movie-vote">Vote average: <span>{{result.vote_average}}</span></p>
      </div>
    </div>
    <div class="bottom-info">
      <p>{{result.overview}}</p>
    </div>
    <hr>
  </div>
  <div class="error" v-if="!results.length && isEnd">Hech narsa topilmadi!</div>
  <div class="loading" v-if="loading && !isEnd">Qidirilmoqda...</div>
  </div>
</template>

<script>
import {mapState} from "vuex"
export default {
  data() {
    return {
      search : "",
      results : [],
      isEnd: false,
      loading: false
    }
  },
  computed : {
    ...mapState("movieState", ["isOpen"])
  },
  methods : {
    async getSearchedMovies() {
      try{
          this.loading = true;
          const apiKey = "fa61e6fa7724edd99048bc5f0b11ae72";
          let res = (
            await fetch(
              `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${this.search}`
            )
          );
          this.isEnd = true;
          this.results = (await res.json()).results;
          this.loading = false
      }
      catch(e) {
        console.log(e)
      }
      
    },
    async getSearchedMoviesInput(e) {
      try{
        if(e.keyCode==13) {
          this.loading = true
          const apiKey = "fa61e6fa7724edd99048bc5f0b11ae72";
          let res = (
            await fetch(
              `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${this.search}`
            )
          );
          this.isEnd = true;
          this.results = (await res.json()).results;
          this.loading = false
      } 
      }
      catch(e) {
        console.log(e)
      }
    }
  },
  watch : {

  },
mounted() {
  
}
}
</script>

<style scoped>
* {
  font-family: "Gilroy";
}
.wrapper {
  margin-top: 0;
}
.title {
  text-align: center;
  margin: 2rem 0;
  color: yellow;
}
.search-form {
  display: block;
  margin: 0 auto;
  padding: 5rem;
  text-align: center;
  transition: all ease-in-out .3s;
}
.search-form.open {
  margin-top: 11rem;
}
.search-input {
  width: 55%;
  font-size: 1rem;
  padding: 0.5rem;
  border-radius: 0.5rem;
  border: 1px solid rgb(0 0 0 / 20%);
  box-shadow: 0 1px 4px 2px rgb(0 0 0 / 20%);
  outline: none;
  display: inline-block;
  margin: 0 1rem;
}
.search-form button {
  cursor: pointer;
  margin: 0 0.5rem;
  display: inline-block;
  font-size: 18px;
  text-transform: uppercase;
  color: #fff;
  background: blue;
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
  border: 1px solid #516552;
  box-shadow: 0 1px 4px 2px rgb(0 0 0 / 20%);
}
.info-container {
  width: 800px;
  margin: 0 auto;
  background: #111;
  padding: 1rem;
}
  .movie-info {
    display: flex;
    margin-top: 2rem;
  }
  .movie-img {
    flex-basis: 75%;
  }
  .movie-img img {
    width: 100%;
    height: 350px;
    margin: 1rem;
    margin-left: 0;
    border-radius: 5px;
  }
  img[type=alt] {
    color: #fff;
  }
  .movie-data {
    margin: 2rem;
  }
  .movie-data p {
    font-size: 1.2rem;
    margin: 1rem;
    color: #fff;
  }
  .movie-data p span {
    color: #f90;
  }
  .movie-title {
    font-size: 2rem !important;
    font-weight: 600;
    margin-bottom: 1.5rem !important;
    color: #fff !important;
  }
  .movie-title.active {
    background: yellow;
  }
  .movie-genre span {
    margin-right: 0.5rem;
    color: #f90;
  }
  .bottom-info {
    margin-bottom: 2rem;
  }
  .bottom-info p {
    font-size: 1.3rem;
    line-height: 2rem;
    color: aliceblue;
  }
  .error {
    text-align: center;
    font-size: 18px;
    margin-top: 3rem;
    color: azure;
  }
  .loading {
    /* display: flex;
    align-items: center;
    flex-direction: column;
    font-size: 22px;
    color: aliceblue; */
    text-align: center;
    font-size: 18px;
    margin-top: 3rem;
    color: azure;
  }
  @media screen and (max-width: 400px) {
    * {
      overflow-x: hidden;
    }
    .wrapper {
      background: #111;
      min-height: 100vh;
    }
    .info-container {
      padding: 0;
    }
    .search-form {
      width: 320px;
      margin: 5rem auto;
      padding: 0.5rem;
      text-align: center;
    }
    .search-input {
      width: 100%;
      margin: 0 auto;
    }
    .search-form button {
      font-size: 1rem;
      padding: 0.5rem;
    }
     .movie-info {
    flex-direction: column;
  }
    .movie-img {
    width: 350px;
    text-align: center;
    margin: 2.2rem 0;
  }
  .movie-img img{
    width: 80%;
    height: 250px;
  }
  .bottom-info {
    width: 320px;
    margin: 0 1rem;
    font-size: 16px;
    text-align: justify;
  }
  .movie-data {
    width: 350px;
  }
  .movie-title {
    margin-left: 0;
  }
  }
</style>