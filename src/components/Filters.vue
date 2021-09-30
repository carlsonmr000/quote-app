<template>
  <div iv class="container">
    <h3>Movie/TV Quotes:</h3>
    <input
      class="search"
      type="text"
      v-model="search"
      placeholder="search quotes"
      @input="filter"
    />
    <!-- <span v-if="filteredQuotes.length === 1">No data</span> -->
    <table class="table">
      <tbody>
        <tr v-for="quote in filteredQuotes" v-bind:key="quote.id">
          <td>{{ quote.source }}</td>
          <td>{{ quote.context }}</td>
          <td>{{ quote.quote }}</td>
          <td>{{ quote.theme }}</td>
        </tr>
      </tbody>
    </table>

    <div>
      <div v-if="displayMore">
        <p class="more">More Quotes Below:</p>
      </div>

      <div v-else>
        <p></p>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "Quotes",
  data: () => ({
    quotes: null,
    search: "",
    displayMore: false,
  }),
  created: function() {
    axios
      .get(
        "https://gist.githubusercontent.com/benchprep/dffc3bffa9704626aa8832a3b4de5b27/raw/quotes.json"
      )
      .then((res) => {
        this.quotes = res.data;
      });
  },
  methods: {
    filter() {
      if (this.search === "" && this.search.length === -1) {
        this.displayMore = false;
        this.filteredQuotes = this.quotes;
      } else {
        this.displayMore = true;
        this.filteredQuotes = [];
        this.quotes.forEach((quote) => {
          if (quote.quote.toLowerCase().includes(this.search.toLowerCase()))
            this.filteredQuotes.push(quote);
          console.log("display status", this.displayMore);
        });
      }
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

.button {
  margin: 5px;
  border-radius: 6px;
  border: none;
  padding: 5px 10px;
  background-color: black;
  color: white;
  font-weight: bold;
}

.more {
  font-weight: bold;
}
</style>
