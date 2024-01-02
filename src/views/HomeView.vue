<script setup>
import axios from 'axios';
import {onMounted, ref} from "vue";

const boutiques = ref([])

const newBoutique = ref({
    name: "",
    description: "",
    address: ""
});


onMounted(async () => {
    await getBoutiques();
})

async function getBoutiques() {
    try {
        const response = await axios.get('http://localhost:1337/api/shops?populate=comments')
        boutiques.value = response.data
        console.log(boutiques)
    } catch (error) {
        console.error(error.message)
    }
}

async function addBoutique() {
    try {
        await axios.post('http://localhost:1337/api/shops', { data: newBoutique.value });
        // Recharger la liste des boutiques après l'ajout
        await getBoutiques();
        // Réinitialiser le formulaire après l'ajout
        resetForm();
    } catch (error) {
        console.error(error.message);
    }
}

function resetForm() {
    newBoutique.value = {
        name: "",
        description: "",
        address: ""
    };
}

</script>

<template>
  <main>
      <div v-for="boutique in boutiques.data" :key="boutique.id" class="boutique">
        <h2>{{ boutique.attributes.name }}</h2>
        <p class="description">{{ boutique.attributes.description }}</p>
        <p>{{ boutique.attributes.address }}</p>
        <p>Nombre de commentaires : {{ boutique.attributes.comments.data.length }}</p>
        <button>
            <router-link :to="'/boutique/' + boutique.id">Consulter</router-link>
        </button>
    </div>

      <div class="ajouter-boutique">
          <h2>Ajouter une boutique</h2>
          <form @submit.prevent="addBoutique">
              <label for="name">Nom de la boutique:</label>
              <input v-model="newBoutique.name" type="text" id="name" required>

              <label for="description">Description:</label>
              <textarea v-model="newBoutique.description" id="description" required></textarea>

              <label for="address">Adresse:</label>
              <input v-model="newBoutique.address" type="text" id="address" required>

              <button type="submit">Ajouter la boutique</button>
          </form>
      </div>
  </main>
</template>
