<script setup>
import axios from 'axios';
import {onMounted, ref} from "vue";

const boutiques = ref([])

onMounted(async () => {
    try {
        const response = await axios.get('http://localhost:1337/api/shops')
        boutiques.value = response.data
        console.log(boutiques)
    } catch (error) {
        console.error(error.message)
    }
})
</script>

<template>
  <main>
      <pre>{{ boutiques.data }}</pre>
    <div v-for="boutique in boutiques.data" :key="boutique.id" class="boutique">
        <h2>{{ boutique.attributes.name }}</h2>
        <p>{{ boutique.attributes.description }}</p>
        <p>Nombre de commentaires : 5</p>
        <button>Consulter</button>
    </div>
  </main>
</template>
