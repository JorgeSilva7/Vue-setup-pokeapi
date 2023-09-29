<script setup>
import { useRouter } from 'vue-router';
import { ref, onMounted } from 'vue';
import axios from 'axios';

const router = useRouter();

const props = defineProps({
  name: {
    type: String,
    required: true,
  },
  url: {
    type: String,
    required: true,
  },
});

const number = ref('#');
const img = ref('');
const abilities = ref([]);

onMounted(async () => {
  number.value = props.url.split('/')[6];
  try {
    const response = await axios.get(props.url);
    img.value = response.data.sprites.front_default;
    abilities.value = response.data.abilities;
  } catch (err) {
    console.log('Error ;(');
  }
});

function showDetails(ability) {
  const abilityId = ability.url.split('/')[6];
  router.push({ name: 'ability', params: { id: abilityId } });
}
</script>

<template>
  <div class="card clickeable" style="width: 18rem">
    <img :src="img" class="card-img-top" />
    <div class="card-body">
      <h5 class="card-title">{{ name }}</h5>
      <p class="card-text">
        Some quick example text to build on the card title and make up the bulk
        of the card's content.
      </p>
      <button
        v-for="a in abilities"
        :key="a.slot"
        @click="showDetails(a.ability)"
        class="btn btn-primary"
      >
        {{ a.ability.name }}
      </button>
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

.clickeable {
  cursor: pointer;
}

.clickeable:hover {
  border: 1px solid red;
}
</style>
