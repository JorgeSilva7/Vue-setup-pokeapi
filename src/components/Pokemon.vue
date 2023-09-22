<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios';

const props = defineProps({
  name: {
    type: String,
    required: true
  },
  url: {
    type: String,
    required: true
  },
})

const number = ref('#');
const img = ref('');
const types = ref([]);

onMounted(async () => {
  number.value = props.url.split('/')[6];
  try{
    const response = await axios.get(props.url);
    img.value = response.data.sprites.front_default;
    types.value = response.data.types;
  } catch(err) {
    console.log('Error ;(')
  }
});
</script>

<template>
  <div class="card" style="width: 18rem;">
  <img :src="img" class="card-img-top">
  <div class="card-body">
    <h5 class="card-title">{{ name }}</h5>
    <p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
    <a v-for="t in types" :key="t.slot" href="#" class="btn btn-primary">{{ t.type.name }}</a>
  </div>
</div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings {

  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
