<template>
  <div id="app">
    <div class="text-center position-relative">
        <InputSearch @input="setParentValueInput" placeholder="Pesquise por um anime"/>

      <transition enter-active-class="blowUp" leave-active-class="blowDown">
        <Loading v-show="loading"/>
      </transition>
    </div>

    <transition enter-active-class="blowUp" leave-active-class="blowDown">
      <div class="anime-search" v-show="isDescriptionActive">
        <span class="anime-search__description">Nenhum anime pesquisado ainda</span>
      </div>
    </transition>

    <main>
      <transition-group
        enter-active-class="blowUp"
        leave-active-class="blowDown"
        tag="div"
        class="card-container"
      >
        <Card
          v-for="(anime, index) in returnedAnime.results"
          :key="index"
          @click="itemClicked"
          :id="index"
        >
          <img :src="anime.image_url" slot="anime-image" />
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
      </transition-group>

      <transition enter-active-class="blowUp" leave-active-class="blowDown">
        <Modal v-show="isModalOpen" @close="isModalOpen = false">
          <div slot="modal-bg" class="modal__bg">
            <img :src="dataAnimeClicked.image_url" alt />
          </div>
          <div slot="modal-info-anime" class="modal__info-anime">
            <div class="modal__content-item">
              <h3>Nome</h3>
              <p>{{dataAnimeClicked.title}}</p>
            </div>
            <div class="modal__content-item">
              <h3>Tipo</h3>
              <p>{{dataAnimeClicked.type}}</p>
            </div>
            <div class="modal__content-item">
              <h3>Episódios</h3>
              <p>{{dataAnimeClicked.episodes | stringEpisodes(dataAnimeClicked.episodes)}}</p>
            </div>
            <div class="modal__content-item modal__content-item--score">
              <i class="fas fa-star"></i>
              <span>{{dataAnimeClicked.score}}</span>
            </div>
            <div class="modal__content-item">
              <h3>Status</h3>
              <p>{{dataAnimeClicked.airing | stringAiring(dataAnimeClicked.airing)}}</p>
            </div>
            <div class="modal__content-item modal__content-item--synopsis">
              <h3>Sinopse</h3>
              <p>{{dataAnimeClicked.synopsis}}</p>
            </div>
          </div>
        </Modal>
      </transition>
    </main>
  </div>
</template>

<script>
import InputSearch from "./components/InputSearch.vue";
import Card from "./components/Card";
import Modal from "./components/Modal";
import Loading from "./components/Loading";

export default {
  name: "App",
  components: {
    InputSearch,
    Card,
    Modal,
    Loading,
  },

  data() {
    return {
      parentValue: "",
      textInputed: "",
      returnedAnime: {},
      dataAnimeClicked: {},
      isModalOpen: false,
      isDescriptionActive: true,
      loading: false
    };
  },

  methods: {
    setParentValueInput(val) {
      this.parentValue = val;
      this.getDataApi();
    },

    itemClicked(target) {
      this.dataAnimeClicked = this.returnedAnime.results[target.id];
      this.isModalOpen = true;
    },

    async getDataApi() {
      this.isDescriptionActive = false;
      this.loading = true;

      try {
        const data = await fetch(
          `https://api.jikan.moe/v3/search/anime?q=${this.parentValue}`
        );
        const jsonData = await data.json();
        this.returnedAnime = jsonData;
      } finally {
        this.loading = false;
      }
    }
  },

  filters: {
    stringEpisodes(value) {
      return value === 0 ? "Desconhecido" : value;
    },

    stringAiring(value) {
      return value ? "Lançando" : "Finalizado";
    }
  }
};
</script>