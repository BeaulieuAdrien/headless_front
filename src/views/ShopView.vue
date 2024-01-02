<script setup>
    import axios from 'axios';
    import {ref, onMounted} from "vue";
    import {useRoute} from "vue-router";

    const boutique = ref()
    const route = useRoute()
    const isMounted = ref(false)

    onMounted(async () => {
        console.log('Component mounted.');
        await getShop()
        isMounted.value = true
    })

    const newComment = ref({
        username: "",
        title: "",
        description: "",
        shop: route.params.id
    });

    async function addComment() {
        try {
            const id = route.params.id;
            console.log(newComment);
            await axios.post(`http://localhost:1337/api/comments`, { data: newComment.value});
            console.log('Comment added');
            await getShop();
        } catch (error) {
            console.error(error.message);
        }
    }

    async function getShop() {
        console.log('here')
        try {
            const id = route.params.id
            const response = await axios.get('http://localhost:1337/api/shops/' + id + '?populate=comments')
            boutique.value = response.data.data
        } catch (error) {
            console.error(error.message)
        }
    }

</script>

<template>
    <main v-if="isMounted">
        <div class="boutique">
            <h2>{{ boutique.attributes.name }}</h2>
            <p>{{ boutique.attributes.description }}</p>
            <p>{{ boutique.attributes.address }}</p>
        </div>

        <div class="commentaires">
            <h2>Commentaires</h2>
            <div v-for="comment in boutique.attributes.comments.data" class="comment-box">
                <p><strong>{{ comment.attributes.username }}</strong></p>
                <h3>{{ comment.attributes.title }}</h3>
                <p>{{ comment.attributes.description }}</p>
            </div>
        </div>

        <div class="ajouter-commentaire">
            <h2>Ajouter un commentaire</h2>
            <form @submit.prevent="addComment">
                <label for="username">Nom d'utilisateur:</label>
                <input v-model="newComment.username" type="text" id="username" required>

                <label for="title">Titre du commentaire:</label>
                <input v-model="newComment.title" type="text" id="title" required>

                <label for="description">Description:</label>
                <textarea v-model="newComment.description" id="description" required></textarea>

                <button type="submit">Ajouter le commentaire</button>
            </form>
        </div>

    </main>
</template>