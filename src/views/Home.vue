<script setup>
import Loading from 'vue-loading-overlay';
import 'vue-loading-overlay/dist/css/index.css';

import { ref } from 'vue';
import axios from 'axios';

const limit = ref(0);
const pokemons = ref([]);
const isLoading = ref(false);

import Pokemon from '../components/Pokemon.vue';

async function getPokemons() {
  isLoading.value = true;
  try {
    const response = await axios.get(
      `https://pokeapi.co/api/v2/pokemon?limit=${limit.value}`
    );
    pokemons.value = response.data.results;
    isLoading.value = false;
  } catch (err) {
    isLoading.value = false;
    console.log(err);
    console.log('Error ;(');
  }
}
</script>

<template>
  <header>
    <loading v-model:active="isLoading" is-full-page />
    <div class="wrapper">
      <input v-model="limit" type="number" required />
      <button class="btn btn-info" @click="getPokemons()">Obtener</button>

      <Pokemon
        v-for="pokemon in pokemons"
        :name="pokemon.name"
        :url="pokemon.url"
        :key="pokemon.name"
      />
    </div>
  </header>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
