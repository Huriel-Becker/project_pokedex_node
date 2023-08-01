<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/pokedex-logo.png" />
      <input
        class="input is-rounded"
        type="text"
        name=""
        id=""
        placeholder="Buscar pokemon"
        v-model="busca"
      />
      <button class="button is-fullwidth is-success" id="buscaBtn" @click="buscar">
        Buscar
      </button>
      <div v-for="(poke, index) in filteredPokemon" :key="poke.url">
        <PokemonViewVue :name="poke.name" :url="poke.url" :num="index + 1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import PokemonViewVue from "./components/PokemonComp.vue";

export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      filteredPokemon: [],
      busca: "",
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        console.log("Pegou a lista de pokemons");
        this.pokemons = res.data.results;
        this.filteredPokemon = res.data.results;
      });
  },
  components: {
    PokemonViewVue,
  },
  methods: {
    buscar: function () {
      this.filteredPokemon = this.pokemons;
      if (this.busca == "" || this.busca == " ") {
        this.filteredPokemon = this.pokemons;
      } else {
        this.filteredPokemon = this.pokemons.filter((pokemon) =>
          pokemon.name.toLowerCase().includes(this.busca.toLowerCase())
        );
      }
    },
  },
  computed: {},
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#buscaBtn {
  margin-top: 2%;
}
</style>
