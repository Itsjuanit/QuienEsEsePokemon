<template>
  <div class="back">
    <h1 v-if="!pokemon">Espere por favor...</h1>

    <div v-else>
      <h1>¿Quién es este Pokémon?</h1>

      <PokemonPicture :pokemon-id="pokemon.id" :show-pokemon="showPokemon" />

      <PokemonOptions :pokemons="pokemonArr" @selection-pokemon="checkAnswer" />

      <template v-if="showAnswer">
        <h2 class="fade-in">{{ message }}</h2>
        <button @click="newGame" class="btn-blue-tr">Nuevo Juego</button>
      </template>
    </div>
    <div class="footer">
      <p>
        Made with ♥ by
        <a href="https://portfolio-itsjuanit.vercel.app/" target="_blank"
          >Itsjuanit</a
        >
      </p>
    </div>
  </div>
</template>

<script>
import PokemonOptions from "@/components/PokemonOptions";
import PokemonPicture from "@/components/PokemonPicture";

import getPokemonOptions from "@/helpers/getPokemonOptions";

export default {
  components: { PokemonOptions, PokemonPicture },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: "",
    };
  },
  methods: {
    async mixPokemonArray() {
      this.pokemonArr = await getPokemonOptions();

      const rndInt = Math.floor(Math.random() * 4);
      this.pokemon = this.pokemonArr[rndInt];
    },
    checkAnswer(selectedId) {
      this.showPokemon = true;
      this.showAnswer = true;

      if (selectedId === this.pokemon.id) {
        this.message = `Correcto, el Pokemon era ${this.pokemon.name}`;
      } else {
        this.message = `Es incorrecto, el Pokemon era ${this.pokemon.name}`;
      }
    },
    newGame() {
      this.showPokemon = false;
      this.showAnswer = false;
      this.pokemonArr = [];
      this.pokemon = null;
      this.message = "";
      this.mixPokemonArray();
    },
  },
  mounted() {
    this.mixPokemonArray();
  },
};
</script>
<style scoped>
.back {
  background: url("../assets/qeep.png");
  width: 100vw;
  height: 100vh;
  position: fixed;
  background-size: 110% 110%;
  background-repeat: no-repeat;
}
div::first-letter {
  text-transform: uppercase;
}
h2::first-letter {
  text-transform: uppercase;
}

.btn-blue-tr {
  border: none;
  font-weight: bold;
  font-size: 1rem;
  text-align: center;
  color: #202125;
  cursor: pointer;
  max-width: 200px;
  width: 100%;
  outline: 1px solid;
  outline-color: rgba(71, 126, 232, 0.5);
  outline-offset: 5px;
  transition: all 600ms cubic-bezier(0.2, 0, 0, 0.8);
}

.btn-blue-tr:hover {
  color: rgba(71, 126, 232, 0.7);
  outline-color: rgba(71, 126, 232, 0);
  outline-offset: 200px;
  font-size: 1.5rem;
}
.footer {
  width: 100%;
  text-align: center;
  position: fixed;
  bottom: 0;
  font-size: 1rem;
  color: #212121;
  font-weight: bold;
}
a {
  color: #fff;
  text-decoration: none;
}
a:hover {
  font-size: 1.2;
}
</style>

