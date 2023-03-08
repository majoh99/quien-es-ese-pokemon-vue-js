<template>
  <div class="page-container d-flex">
    <div class="col pokemon-img d-flex items-center justify-center">
      <h2 v-if="!pokemon">Espere, por favor...</h2>
      <div v-else class="d-flex items-center justify-center picture-container">
        <img class="logo-mobile" src="@/assets/quien-es-ese-pokemon.webp" alt="¿Quién es ese Pokemon?">
        <PokemonPicture 
          :pokemon-id="pokemon.id" 
          :show-pokemon="showPokemon" 
        />
      </div>
      
    </div>
    <div v-if="pokemon" class="col pokemon-options bg-red">
      <img class="logo" src="@/assets/quien-es-ese-pokemon.webp" alt="¿Quién es ese Pokemon?">
      <PokemonOptions 
        :pokemons="pokemonArr" 
        @selection-pokemon="checkAnswer" 
        :status-option="status"
        :pokemon-id="pokemon.id"
        :is-correct="isCorrect"
       />
      <template v-if="showAnswer">
        <p class="fade-in text-white">{{ message }}</p>
        <div class="d-flex justify-center">
          <button class="btn-newGame d-flex items-center text-white" @click="newGame">
            <img src="@/assets/icons/play_circle.svg" alt="play_circle">
            Volver a jugar
          </button>
        </div>
      </template>
    </div>
  </div>
  <!-- <h2 v-if="!pokemon">Espere por favor...</h2>
  <div v-else>
    <img class="logo" src="@/assets/quien-es-ese-pokemon.webp" alt="¿Quién es ese Pokemon?">
    <PokemonPicture :pokemon-id="pokemon.id" :show-pokemon="showPokemon" />
    <PokemonOptions :pokemons="pokemonArr" @selection-pokemon="checkAnswer" :status-option="status" />
    <template v-if="showAnswer">
      <h2 class="fade-in">{{ message }}</h2>
      <button class="btn-newGame" @click="newGame">Nuevo Juego</button>
    </template>
  </div> -->
</template>

<script>
import PokemonPicture from '@/components/PokemonPicture'
import PokemonOptions from '@/components/PokemonOptions'

import getPokemonOptions from '@/helpers/getPokemonOptions'

export default {
  name: 'PokemonPage',
  components: { PokemonPicture, PokemonOptions },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: '',
      status: false,
      isCorrect: null,
    }
  },
  methods: {
    async mixPokemonArray() {
      this.pokemonArr = await getPokemonOptions()
      const rndInt = Math.floor( Math.random() * 4 )
      this.pokemon = this.pokemonArr[ rndInt ]
    },
    checkAnswer( selectedId ) {
      this.showPokemon = true
      this.showAnswer = true
      this.pokemon.id === selectedId
        ? (this.isCorrect = true)
        : (this.isCorrect = false)

      if (selectedId === this.pokemon.id) {
        const name = this.pokemon.name
        this.message = `Correcto, es ${ name.charAt(0).toUpperCase() + name.slice(1) }`
        this.status = true

      } else {
        const name = this.pokemon.name
        this.message = `Oops, era ${ name.charAt(0).toUpperCase() + name.slice(1) }`
        this.status = true
      }
    },
    newGame() {
      this.showPokemon = false,
      this.showAnswer = false,
      this.pokemonArr = [],
      this.pokemon = null,
      this.status = false,
      this.isCorrect = null;
      this.mixPokemonArray()
    }
  },
  mounted() {
    this.mixPokemonArray()
  }
}
</script>

<style scoped>
.bg-red{
  background-color: var(--background-color);
}

.btn-newGame{
  background-color: transparent;
  border-radius: 5px;
  border: none;
  cursor: pointer;
  font-size: 16px;
  margin-bottom: 10px;
  padding: 12px 6px;
}
</style>

