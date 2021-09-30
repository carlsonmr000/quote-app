<template>
  <div iv class="container">  
    <table class="table">
      <tbody>
        <tr v-for="quote in partialQuotes" v-bind:key="quote.id">
          <td>{{ quote.source }}</td>
          <td>{{ quote.context }}</td>
          <td>{{ quote.quote }}</td>
          <td>{{ quote.theme }}</td>
        </tr>
      </tbody>
    </table>

    <div class="button-container">
      <button class="button" v-on:click="onNextPage('Prev')">Previous</button>
      <button class="button" v-on:click="onNextPage('Next')">Next</button>
      <button class="button" v-on:click="onMovieClick">Movie</button>
      <button class="button" v-on:click="onGamesClick">Game</button>
      
    </div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "Quotes",
  data: () => ({
    quotes: null,
    search: '',
    partialQuotes: null,
    offset: 0,
    limit: 5,
    games: [],
    movies: [],
    showGames: false,
    showMovies: false,
  }),
  created: function() {
    axios
      .get(
        "https://gist.githubusercontent.com/benchprep/dffc3bffa9704626aa8832a3b4de5b27/raw/quotes.json"
      )
      .then((res) => {
        // this.quotes = res.data
        const partial = [];
        for (let i = 13; i < 18; i++) {
          partial.push(res.data[i]);
        }

        const movies = [];

        for (let i = 0; i < res.data.length; i++) {
          if (res.data[i].theme === "movies") {
            movies.push(res.data[i]);
          }
        }

        const games = [];

        for (let i = 0; i < res.data.length; i++) {
          if (res.data[i].theme === "games") {
            games.push(res.data[i]);
          }
        }
        this.games = games;

        this.movies = movies;

        this.partialQuotes = partial;
        this.quotes = res.data;
      });
},
  methods: {

    onNextPage(direction) {
      if (direction === "Next") {
        this.offset = this.offset + 5;
        this.limit = this.limit + 5;
      } else if (direction === "Prev") {
        this.offset = this.offset - 5;
        this.limit = this.limit - 5;
      }

      let allQuotes = this.quotes;
      if (this.showGames) {
        allQuotes = this.games;
      }
      if (this.showMovies) {
        allQuotes = this.movies;
      }
      if (this.showGames === true && this.showMovies === true) {
        allQuotes = this.quotes;
      }

      if (this.limit > allQuotes.length) {
        this.limit = 5;
        this.offset = 0;
      }

      const newPartials = [];

      if (this.showGames === true && this.showMovies === false) {
        this.partialQuotes = this.games;
      } else {
        for (let i = this.offset; i <= this.limit; i++) {
          if (allQuotes[i]) {
            newPartials.push(allQuotes[i]);
          }
        }
      }

      this.partialQuotes = newPartials;
 
    },

    onMovieClick() {
      this.showMovies = !this.showMovies;
      this.partialQuotes = this.movies.slice(0, 5);
      this.limit = 5;
      this.offset = 0;
    },

    onGamesClick() {
      this.showGames = !this.showGames;
      this.partialQuotes = this.games;
      this.limit = 5;
      this.offset = 0;
    },
  },
};
</script>
<style>
h3 {
  margin-bottom: 5%;
}

.search {
  margin: 10px;
  border-radius: 6px;
  width: 86vw;
}

.button-container {
  display: flex;
  justify-content: center;
}

.button {
  margin: 5px;
  border-radius: 6px;
  border: none;
  padding: 5px 10px;
  background-color: black;
  color: white;
  font-weight: bold;
}
</style>
