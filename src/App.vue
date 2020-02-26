<template>
  <div id="app">
    <div class="text-center">
      <InputSearch 
        @keyUpVal="setParentValueInput"
        placeholder="Pesquise por um anime"
        class="search"
      />
    </div>

    <main>
      <div class="card-container">
        <Card
          v-for="(anime, index) in returnedAnime.results" :key="index"
          @click="itemClicked"
          :id="index"
          >
          <img :src="anime.image_url" slot="anime-image"/>
          <div slot="anime-name">
            <h3>Nome</h3>
            <p>{{anime.title}}</p>
          </div>
          <div slot="anime-synopsis">
            <h3>Sinopse</h3>
            <p>{{anime.synopsis}}</p>
          </div>
          <h3 slot="anime-seemore">Veja mais</h3>    
        </Card>
      </div>

        <transition enter-active-class="blowUp" leave-active-class="blowDown">
          <div class="modal" v-if="isModalOpen">
            <div class="modal__overlay">
              <button 
                class="modal__close"
                @click="isModalOpen = false"
              >
                X
              </button>
              <transition enter-active-class="blowzinhoup" leave-active-class="blowzinhodown">
                <div class="modal__content">
                  <div class="modal__header"></div>
                  <div class="modal__body"></div>
                  <div class="modal__footer"></div>
                </div>
              </transition>
            </div>
          </div>
        </transition>

    </main>

  </div>
  
</template>

<script>
import InputSearch from "./components/InputSearch.vue";
import Card from "./components/Card";

export default {
  name: "App",
  components: {
    InputSearch,
    Card
  },

  data() {
    return {
      parentValue: "",
      textInputed: "",
      returnedAnime: {},
      dataAnimeClicked: {},
      isModalOpen: false,
    };
  },

  methods: {
    setParentValueInput(val) {
      this.parentValue = val;
      this.getDataApi()
    },

    itemClicked(target) {
      console.log(target.id)
      this.dataAnimeClicked = this.returnedAnime.results[target.id]
      this.isModalOpen = true
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