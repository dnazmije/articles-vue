<script setup>
import { reactive, ref, onMounted} from 'vue'
import ArticleCard from './ArticleCard.vue'

    const articlesToFavorites = ref([]);


    const getFavoriteArticles = () => {
        const favouriteArticles = JSON.parse(localStorage.getItem("articles"));
        if (favouriteArticles) {
            articlesToFavorites.value = favouriteArticles;
        }
    };

    onMounted( () => {
        getFavoriteArticles();
    })

</script>

<template>
    <div>
        <h1>Favorites</h1>
        <br>
        <p>List of favorite articles</p>
        <ul v-for="(article, index) in articlesToFavorites" :key=index>
            <ArticleCard :article="article" />
        </ul>

        <div class="card" v-if=!articlesToFavorites> No favorites yet.</div>
    </div>
</template>

<style scoped>

</style>