<template>
  <div class="pokemon-container">
    <img 
    v-if="!showPokemon"
    :src="imgSrc"
    class="hidden-pokemon"
    alt="pokemon"
    >
    <img 
      v-else
      :src="imgSrc"
      class="fade-in"
      alt="pokemon"
    >
  </div>
</template>
<script>
export default {
  name: 'PokemonPicture',
  props: {
    pokemonId: {
      type: Number,
      required: true
    },
    showPokemon: {
      type: Boolean,
      required: true,
      default: false,
    }
  },
  computed: {
    imgSrc() {
      return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${this.pokemonId}.svg`
    }
  }
}
</script>
<style scoped>
/* Pokemon Picture */
.pokemon-container {
  height: 200px;
  display: flex;
  justify-content: center;
}
img {
  height: 200px;
  position: absolute;
  user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  -webkit-user-drag: none;
  -webkit-user-select: none;
}
.hidden-pokemon {
  filter: contrast(0) sepia(100%) hue-rotate(190deg) saturate(1675%) brightness(60%);
}

@media screen and (max-width: 769px) {
  img {
    height: 180px;
  }
}
</style>