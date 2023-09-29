<script setup>
import Loading from 'vue-loading-overlay';
import 'vue-loading-overlay/dist/css/index.css';
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';

const isLoading = ref(false);

const pokemonData = ref(null);
const effect = ref(null);

const route = useRoute();
const id = route.params.id;

onMounted(async () => {
  isLoading.value = true;
  try {
    const response = await axios.get(`https://pokeapi.co/api/v2/ability/${id}`);
    pokemonData.value = response.data;
    effect.value = response.data.effect_entries.find(
      (effect) => effect.language.name === 'en'
    );
    isLoading.value = false;
  } catch (err) {
    isLoading.value = false;
    console.log('Error ;(');
  }
});
</script>

<template>
  <div>
    <loading v-model:active="isLoading" is-full-page />
    <h1>Pokemon detail</h1>
  </div>
  <div v-if="pokemonData">
    <ul class="list-group">
      <li class="list-group-item" v-for="n in pokemonData.names" :key="n.name">
        {{ n.name }}
      </li>
    </ul>
    <div class="alert alert-info mt-3" role="alert" v-if="effect">
      {{ effect.effect }}
    </div>
  </div>
</template>
