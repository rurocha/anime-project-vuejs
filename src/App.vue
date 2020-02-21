<template>
  <div id="app">
    <div>
      <InputSearch 
        @keyUpVal="setParentValueInput"
        placeholder="Pesquise por um anime"
      />

      <div class="card" v-for="(anime, index) in returnedAnime.results" :key="index">
        <p>{{anime.title}}</p>
        <img :src="anime.image_url" />
      </div>
    </div>
  </div>
</template>

<script>
import InputSearch from "./components/InputSearch.vue";

export default {
  name: "App",
  components: {
    InputSearch
  },

  data() {
    return {
      parentValue: "",
      textInputed: "",
      returnedAnime: {}
    };
  },

  methods: {
    setParentValueInput(val) {
      this.parentValue = val;
      this.getDataApi()
    },

    async getDataApi() {
      const data = await fetch(
        `https://api.jikan.moe/v3/search/anime?q=${this.parentValue}`
      );
      const jsonData = await data.json();
      this.returnedAnime = jsonData;
    }
  }
};
</script>

<style style lang="scss">
  body {
    text-align: center;
  }
</style>