<template>
    <pokemon-cards :pokemons="starters"
                   @pokemonClicked="fetchEvolutions"
                   :selectedId="selectedId"
    />
    <pokemon-cards :pokemons="evolutions"/>
    
</template>

<script>
import PokemonCards from './PokemonCards'

const api = ' https://pokeapi.co/api/v2/pokemon'
const STARTER_IDS = [1,4,7]

export default {
  name: 'Cards',
  components: {
      PokemonCards
  },
  async created(){
      const starters = await this.fetchData(STARTER_IDS)
      this.starters = starters
  },
  data(){
      return {
          starters: [],
          evolutions: [],
          selectedId: null
      }
  },
  methods: {
      async fetchEvolutions(pokemon){
          this.selectedId = pokemon.id
          this.evolutions = await this.fetchData([pokemon.id + 1, pokemon.id + 2])
         
      },
      async fetchData(ids){
          const responses = await Promise.all(ids.map(id => window.fetch(`${api}/${id}`)))
          const data = await Promise.all(responses.map(res => res.json()))
          return data.map(datum => ({
              id: datum.id,
              name: datum.name,
              sprite: datum.sprites.other['official-artwork'].front_default,
              types: datum.types.map(type => type.type.name)
          }))
      }
  }
}
</script>

<style scoped>
</style>
