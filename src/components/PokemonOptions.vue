<template>
  <div class="options-container d-flex justify-center items-center">
    <ul>
      <li 
        v-for="pokemon in pokemons" 
        :key="pokemon.id"
        :disable="statusOption"
        :class="addClass(pokemon.id)"
        @click="selectionPokemon(pokemon.id)">
        {{ pokemon.name }}
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  name: 'PokemonOptions',
  props: {
    pokemons: {
      type: Array,
      required: true
    },
    statusOption: {
      type: Boolean,
      default: false
    },
    isCorrect: {
      type: Boolean,
      default: null
    },
    pokemonId: {
      type: Number,
      default: null,
      required: true
    }
  },
  data(){
    return{
      userSelection: null
    }
  },
  methods: {
    selectionPokemon(id) {
      this.$emit('selectionPokemon', id)
      this.userSelection = id;
    },
    addClass(id){
      if (this.isCorrect === null) return;

      if (this.isCorrect) {
        return this.userSelection === id ? "correct active" : "incorrect";
      } else {
        return this.userSelection === id
          ? "incorrect active"
          : this.pokemonId === id
          ? "correct"
          : "incorrect";
      }
    }
  }
}
</script>
<style scoped>
/* Pokemon Options */
.options-container ul {
  list-style-type: none;
  margin-top: 24px;
  padding-left: 0;
}

.options-container li{
  background-color: white;
  border: 2px solid var(--blue-color);
  border-radius: 5px;
  margin-bottom: 10px;
  padding: 12px;
  text-transform: capitalize;
  width: 250px;
}
.options-container li[disable = false] {
  color: var(--blue-color);
  cursor: pointer;
}

.options-container li[disable = true] {
  cursor: not-allowed;
  pointer-events: none;
}

.options-container li[disable = false]:hover {
  background-color: rgba(0, 0, 0, 0.05);
  border: 2px solid white;
  color: white;
}

.options-container li.correct{
  background-color: var(--correct-color);
  border: 2px solid var(--correct-color);
  color: var(--correct-active-color);
}
.options-container li.incorrect{
  background-color: var(--incorrect-color);
  border: 2px solid var(--incorrect-color);
  color: var(--incorrect-active-color);
}

.options-container li.correct.active {
  background-color: var(--correct-active-color);
  border: 2px solid var(--correct-active-color);
  color: var(--correct-color);
}

.options-container li.incorrect.active {
  background-color: var(--incorrect-active-color);
  border: 2px solid var(--incorrect-active-color);
  color: white;
}
</style>