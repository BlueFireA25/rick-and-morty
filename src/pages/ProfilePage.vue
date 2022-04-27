<template>
  <div class="content">
    <div class="character-appearance">
      <img :src="character.image" alt="">
      <label>{{ character.gender }}</label>
    </div>
    <div class="character-information">
      <div class="name-information">
        <label id="character-id">Id: {{ character.id }}</label>
        <h2>{{ character.name }}</h2>
        <label id="character-status">{{ character.status }}</label>
      </div>
      <div class="specie-information">
        <label>Especie:</label><br>
        <span>{{ character.species }}</span>
      </div>
      <div class="origin-information">
        <label>Origen:</label><br>
        <span>{{origin.name}}</span>
      </div>
      <div class="location-information">
        <label for="">Ubicación:</label><br>
        <span>{{location.name}}</span>
      </div>
      <div class="created-information">
        <label>Este personaje fue creado el:</label><br>
        <span>{{ character.created }}</span>
      </div>
    </div>
  </div>

  <div class="episodes">
    <h3>Episodios / {{episodes.length}}</h3>
    <div class="episodes-information" >
      <q-card class="my-card" v-for="episode in episodes" :key="episode.id">
        <q-card-section class="card-section">
          <div class="text-episode">{{episode.episode}}</div>
          <div class="text-episode_name">{{episode.name}}</div>
          <div class="text-episode_airdate">Al aire: {{episode.air_date}}</div>
          <div class="text-subtitle">Pesonajes</div>
        </q-card-section>
        <div class="characters-information">
          <div class="text-character" v-for="charac in characters_episodes.slice(0, 3)" :key="charac.id">
            <span>{{ charac.name }}</span>
          </div>
        </div>
      </q-card>
    </div>
  </div>

</template>

<script>
  import axios from 'axios'

  export default {
    name: 'ProfilePage',
    components: {

    },
    data() {
      return {
        character: {},
        origin: {},
        location: {},
        episodes: [],
        characters_episodes: [],
      }
    },
    created() {
      this.fetch()
    },
    methods: {
      // Obtener los datos del personaje por ruta.
      fetch() {
        axios.get('https://rickandmortyapi.com/api/character/'+this.$route.params.id)
        .then(res => {
          this.character = res.data;
          this.origin = this.character.origin;
          this.location = this.character.location;

          // Agrega todos los episodios donde aparece el personaje.
          for (let i = 0; i < this.character.episode.length; i++) {
             axios.get(this.character.episode[i])
             .then(res2 => {
                this.episodes.push(res2.data);

                // Agregar los personakes que aparecen en cada episodio
                for(let j = 0; j < this.episodes.length; j++){
                  axios.get(this.episodes[i].characters[j])
                  .then(res3 => {
                    this.characters_episodes.push(res3.data);
                  })
                }
            })
          }
        })
        .catch (e => {
          console.log(e)
        })
      },
    }
  }
</script>

<style scoped>
  .content {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .character-appearance {
    margin: 72px 32px 0 0
  }

  .character-appearance img {
    width: 329px;
    height: 329px;
    border: 1px solid #00310B;
    box-sizing: border-box;
    box-shadow: 0px 0px 0px 6px #D1D94B;
    border-radius: 8px;
  }

  .character-appearance label {
    font-weight: 700;
    font-size: 20px;
    line-height: 27px;
    margin: 16px 0 0 0;
    display: block;
    text-align: center;
  }

  #character-id {
    font-weight: 500;
    font-size: 20px;
    line-height: 27px;
    color: rgba(0, 49, 11, 0.5);
  }

  .name-information h2 {
    margin: 0;
    padding: 0;
    font-weight: 500;
    font-size: 48px;
    color: #00310B;
  }

  #character-status {
    font-weight: 500;
    font-size: 32px;
    line-height: 44px;
    color: #D1D94B;
  }

  .specie-information label{
    font-weight: 500;
    font-size: 14px;
    line-height: 19px;
    color: rgba(0, 49, 11, 0.5);
  }

  .specie-information span{
    font-weight: 500;
    font-size: 20px;
    line-height: 27px;
    color: #16ACC9;
  }

  .origin-information label{
    font-weight: 500;
    font-size: 14px;
    line-height: 19px;
    color: rgba(0, 49, 11, 0.5);
  }

  .origin-information span{
    font-weight: 500;
    font-size: 20px;
    line-height: 27px;
    color: #FF8A00;
  }

  .location-information label{
    font-weight: 500;
    font-size: 14px;
    line-height: 19px;
    color: rgba(0, 49, 11, 0.5);
  }

  .location-information span{
    font-weight: 500;
    font-size: 20px;
    line-height: 27px;
    color: #FF8A00;
  }

  .created-information label{
    font-weight: 500;
    font-size: 14px;
    line-height: 19px;
    color: rgba(0, 49, 11, 0.5);
  }

  .created-information span{
    font-weight: 500;
    font-size: 20px;
    line-height: 27px;
    color: #FF8A00;
  }

  .episodes h3 {
    font-weight: 300;
    font-size: 48px;
    line-height: 65px;
    color: #16ACC9;
    text-align: center;
  }

  .episodes-information {
    display: grid;
    grid-template-columns: auto auto;
    gap: 25px 19px;
    justify-content: center;
  }

  .my-card {
    width: 502px;
    height: 170px;
    background-color: #FFFFFF;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.16);
    border-radius: 8px;
    margin: 24px 20px;
  }

  .my-card:hover {
    transform: translateY(-10px);
    transition: .4s;
  }

  .text-episode {
    color: #D1D94B;
    font-weight: 700;
    font-size: 16px;
    line-height: 22px;
  }

  .text-episode_name {
    color: #00310B;
    font-weight: 500;
    font-size: 22px;
    line-height: 22px;
  }

  .text-episode_airdate {
    color: #00310B;
    font-weight: 500;
    font-size: 14px;
    line-height: 16px;
  }

  .text-subtitle {
    color: #16ACC9;
    font-weight: 500;
    font-size: 12px;
    line-height: 16px;
    margin: 4px 0 4px 0;
  }

  .text-character {
    display:inline-block;
    text-align: center;
    background: #FFFFFF;
    border: 1px solid #16ACC9;
    box-sizing: border-box;
    border-radius: 32px;
    width: 100px;
  }

  .text-character span {
    margin: 16px 8px;
    color: #16ACC9;
    font-weight: 500;
    font-size: 12px;
    line-height: 16px;
  }

  .characters-information {
    display: flex;
    gap: 0 8px;
    margin-left: 16px;
  }

  @media only screen and (max-width: 1109px) {
    .episodes-information {
      grid-template-columns: auto;
    }
  }
</style>
