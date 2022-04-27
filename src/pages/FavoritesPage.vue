<template>
  <div class="buscador">
    <q-input rounded outlined v-model="search" label="Busca tu personaje favorito" class="input-search">
        <template v-slot:append>
          <q-icon name="search" />
        </template>
    </q-input>
  </div>

  <h2>Favoritos</h2>

  <div class="content">
    <router-link :to="{name:'ProfilePage', params:{id:character.id}}" class="content-characters" v-for="character in filterSearch" :key="character.id">
      <div class="character">
        <img :src="character.image" alt="">
        <div class="info-character">
          <span>Episodio 1</span>
          <span class="character-name">{{ character.name }}</span>
        </div>
        <div class="favorite">
          <q-rating
            v-model="model1"
            max="1"
            size="2.563em"
            color="red-5"
            icon="favorite_border"
            icon-selected="favorite"
            icon-half="favorite_half"
          />
        </div>
      </div>
    </router-link>

  </div>

  <div class="pagination">
    <button v-on:click="changePage( page - 1 )">Anterior</button>
    <span>{{ page }}</span>
    <button v-on:click="changePage( page + 1 )">Siguiente</button>
  </div>
</template>

<script>

  import { ref } from 'vue'
  import axios from 'axios'

  export default {
    name: 'FavoritesPage',
    components: {

    },
    data() {
      return {
        search: '',
        characters: [],
        episodes: [],
        page: 1,
        pages: 1
      }
    },
    setup () {
      const model1 = ref(0)

      return {
        model1,

        resetModels () {
          model1.value = 3.5
        }
      }
    },
    created() {
      this.fetch();
    },
    methods: {
      fetch() {
        const params = {
          page: this.page
        }

        axios.get('https://rickandmortyapi.com/api/character/', { params })
        .then(res => {
          this.characters = res.data.results;

          for (let i = 0; i < this.characters.length; i++) {
            axios.get(this.characters[i].episode[0])
              .then(res2 => {
                this.episodes.push(res2.data);
              })
          }
          this.pages = res.data.info.pages;
        })
        .catch (e => {
          console.log(e)
        })
      },
      changePage(page) {
        this.page = (page <= 0 || page > this.pages) ? this.page : page;
        this.fetch();
      }
    },
    computed: {
      filterSearch() {
        return this.characters.filter(post => {
          return post.name.toLowerCase().includes(this.search.toLowerCase())
        })
      }
    }
  }
</script>

<style scoped>
  .buscador {
    display: flex;
    justify-content: center;
    margin-bottom: 48px;
  }

  .input-search {
    width: 589px;
    height: 56px;
  }

  .content {
    display: grid;
    grid-template-columns: auto auto auto;
    gap: 25px 19px;
    justify-content: center;
  }

  .content-characters {
    width: 328px;
    height: 425px;
    border: 3px solid #16ACC9;
    border-radius: 16px;
    text-decoration: none;
  }

  .content-characters:hover {
    transform: translateY(-10px);
    transition: .5s;
  }

  .character img {
    width: 100%;
    height: 280px;
    border-radius: 16px 16px 0 0;
    margin-bottom: 16px;
  }

  h2 {
    text-align: center;
    color: #16ACC9;
  }

  .info-character{
    display: flex;
    flex-direction: column;
  }

  .info-character span {
    margin: 0 16px 16px;
  }

  .character-episode {
    color: #16ACC9;
    font-weight: 500;
    font-size: 16px;
    line-height: 22px;
  }

  .character-name {
    color: #00310B;
    font-weight: 700;
    font-size: 20px;
    line-height: 27px;
  }

  .pagination {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 20px;
    margin: 20px 0;
  }

  .pagination button {
    padding: 10px 15px;
    border-radius: 20px;
    color: #FFFFFF;
    background-color: #16ACC9;
    border: 3px solid #00310B;
    cursor: pointer;
    font-weight: bold;
  }

  .pagination button:hover {
    transform: translateY(-3px);
    transition: .5s;
  }

  .pagination span {
    padding: 10px 15px;
    color: #FFFFFF;
    background-color: #16ACC9;
    border-radius: 20px;
    border: 3px solid #00310B;
    font-weight: bold;
  }

  .favorite {
    float: right;
    text-align: right;
    margin: -20.2px 21.57px 0;
  }

  @media only screen and (max-width: 1020px) {
    .content {
      grid-template-columns: auto auto;
    }
  }

  @media only screen and (max-width: 705px) {
    .content {
      grid-template-columns: auto;
    }
  }
</style>
