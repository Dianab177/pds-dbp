<template>
  <div>
    <h2 v-if="!pokemon">Espere por favor</h2>

    <div v-else>
      <h1>¿Quién es este pokémon?</h1>
 
      <PokemonPicture 
                      :pokemonId="pokemon.id" 
                      :showPokemon="showPokemon" />

      <PokemonOptions 
                      :pokemons="pokemonArr" 
                      @selectionPokemon="checkAnswer" />
    </div>

    <template v-if="showAnswer">
      <h2 class="fade-in">{{ message }}</h2>
      <button @click="newGame">
        Nuevo juego
      </button>
    </template>

  </div>

</template>

<script>
import PokemonOptions from "../components/PokemonOptions.vue";
import PokemonPicture from "../components/PokemonPicture.vue";
import getPokemonOptions from "../helpers/getPokemonOptions";

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
      this.pokemonArr = await getPokemonOptions()

      const rndInt = Math.floor(Math.random() * 4)
      this.pokemon = this.pokemonArr[rndInt]
    },
    checkAnswer(pokemonId) {
      this.showPokemon = true
      this.showAnswer = true

      if (pokemonId === this.pokemon.id) {
        this.message = `¡Enhorabuena! acertaste es ${this.pokemon.name}`;
      } else {
        this.message = `¡Lo siento!, era ${this.pokemon.name}`;
      }
    },
    newGame() {
      this.showPokemon = false;
      this.showAnswer = false;
      this.pokemonArr = [];
      this.pokemon = null;
      this.mixPokemonArray();
    }
  },
  mounted() {
    this.mixPokemonArray();
  }

}
</script>