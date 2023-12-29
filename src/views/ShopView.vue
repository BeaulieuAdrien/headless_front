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
    </main>
</template>