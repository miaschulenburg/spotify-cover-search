<template>
  <div id="app">
    <div class="background-image"></div>
    <div class="search-content">
      <input v-model="query" @keyup.enter="searchQuery" spellcheck="false" placeholder="Search for an album.">
      <div class="result-list" v-if="data.length > 0">
        <search-result v-for="item of data" :key=item.id :data="item"></search-result>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios"
import SearchResult from './components/SearchResult.vue'

export default {
  name: 'App',
  components: {
    SearchResult
  },
  data: function () {
    return {
      query: "",
      data: [],
      background: ""
    }
  },
  methods: {
    searchQuery: function () {
      axios.get(`https://spotify-cover-search.onrender.com/${this.query}`)
      .then(res => {
        this.data = res.data.albums.items;
        if (this.data.length > 0)
          this.background = this.data[0].images[0].url;
      });
    }
  }
}
</script>

<style>
html {
    margin: 0px;
}

body {
    margin: 0px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #1ecd97;
}

.background-image:before {
  content: "";
  position: fixed;
  left: 0;
  z-index: 2;
  width: 100%;
  height: 100%;
  backdrop-filter: blur(100px) opacity(0.7);
  -webkit-backdrop-filter: blur(100px) opacity(0.7);
  pointer-events: none;
}

.background-image {
  position: fixed;
  height: 100%;
  width: 100%;
  background-position: center center;
  background-size: cover;
  background-attachment: fixed;
  z-index: -1;
  background-color: #555;
}

.search-content {
  margin: auto;
  max-width: 680px;
}

input {
  width: calc(100% - 4 * 20px - 2 * 2px);
  height: 40px;
  padding: 10px 20px;
  margin: 20px 20px;
  margin-top: 100px;
  font-size: 25px;
  border: 2px solid rgba(0, 0, 0, 0);
  background-color: #000;
  border-radius: 5px;
  color: #1ecd97;
  transition: border 0.25s ease;
  box-shadow: 0px 0px 5px 1px #222;
}

input:focus {
  outline: none;
  border: 2px solid #1ecd97;
}

.result-list {
  margin: 0px 20px;
}
</style>
