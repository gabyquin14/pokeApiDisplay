<template>
  <section class="home">
    <div class="home-nav">
      <SearchPokemon
        @pokemonSearch="(data) => (pokemons.results = [data])"
        @clear="fetchPokemon"
      />
      <VPagination
        @pagination="fetchPaginationPokemon"
        :next="pokemons.next"
        :prev="pokemons.previous"
      />
    </div>
    <div v-if="loading">
      <img src="../assets/images/loading.gif" alt="" />
    </div>
    <div class="pokemon-display" v-if="pokemons.results.length > 0">
      <div v-for="pokemon in pokemons.results" :key="pokemon.name">
        <PokemonCard :pokemon="pokemon" :pokemonUrl="pokemon.url" />
      </div>
    </div>
  </section>
</template>

<script>
import axios from "axios";
import PokemonCard from "../components/pokemonCard/PokemonCard.vue";
import VPagination from "../components/pagination/VPagination.vue";
import SearchPokemon from "../components/searchPokemon/SearchPokemon.vue";
export default {
  components: { PokemonCard, VPagination, SearchPokemon },
  data() {
    return {
      pokemons: [],
      loading: false,
    };
  },
  async created() {
    await this.fetchPokemon();
  },
  methods: {
    async fetchPokemon() {
      this.loading = true;
      try {
        const response = await axios.get(
          "https://pokeapi.co/api/v2/pokemon/?limit=20&offset=0"
        );
        this.pokemons = response.data;
        this.loading = false;
      } catch (error) {
        this.$router.push("/not-found");
      }
    },
    async fetchPaginationPokemon(url) {
      try {
        const response = await axios.get(url);
        this.pokemons = response.data;
      } catch (error) {
        this.$router.push("/not-found");
      }
    },
  },
  computed: {},
};
</script>

<style scoped>
.home {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  flex-wrap: wrap;
  align-items: center;
  flex-wrap: wrap;
  row-gap: 4rem;
  margin: 1rem auto 0;
  padding: 4rem;
  width: 100%;
}
.pokemon-display {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  row-gap: 4rem;
  column-gap: 4rem;
}

.home-nav {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  column-gap: 6rem;
  row-gap: 2rem;
  width: 40%;
}

@media screen and (min-width: 600px) {
  .home-nav {
    flex-direction: row;
  }
}
</style>
