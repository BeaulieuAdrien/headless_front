<script setup>
import axios from 'axios';
import {onMounted, ref} from "vue";

const boutiques = ref([])

onMounted(async () => {
    try {
        const response = await axios.get('http://localhost:1337/api/shops?populate=comments')
        boutiques.value = response.data
        console.log(boutiques)
    } catch (error) {
        console.error(error.message)
    }
})
</script>

<template>
  <main>
      <pre>{{ boutiques }}</pre>
      <div v-for="boutique in boutiques.data" :key="boutique.id" class="boutique">
        <h2>{{ boutique.attributes.name }}</h2>
        <p class="description">{{ boutique.attributes.description }}</p>
        <p>{{ boutique.attributes.address }}</p>
        <p>Nombre de commentaires : {{ boutique.attributes.comments.data.length }}</p>
        <button>
            <router-link :to="'/boutique/' + boutique.id">Consulter</router-link>
        </button>
    </div>
  </main>
</template>
