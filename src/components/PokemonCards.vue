<template>
    <div class="cards">
        <card v-for="pokemon in pokemons" 
              :key="pokemon.id" 
              @click="click(pokemon)"
              :class="{opace: selectedId && pokemon.id !== selectedId}"
              class="cardS"
              >
            <template v-slot:title>
                {{pokemon.name}} #{{pokemon.id }}
            </template>
            <template v-slot:content>
                <img :src="pokemon.sprite"/>
            </template>
            <template v-slot:description>
                <div v-for="type in pokemon.types" :key="type.id">
                    {{type}}
                </div>
            </template>
        </card>
    </div>
</template>

<script>
import Card from './card'

export default {
  name: 'Cards',
  components: {
      Card
  },
  props: {
      pokemons: {
          type: Array
      },
      selectedId: {
          type: Number
      }
  },
  methods: {
      click(pokemon){
          this.$emit('pokemonClicked', pokemon)
      }
  }
}
</script>

<style scoped>

.cards {
    display: flex;
}
.opace {
    opacity: 0.5;
}
img {
    max-width: 100%;
}
.cardS:hover {
    opacity:1.0;
}
</style>
