<script setup>
    import { ref, onMounted, reactive} from 'vue'
    import axios from 'axios'
    import ArticleCard from './ArticleCard.vue'
    import SearchBar from './SearchBar.vue';

    const articles = ref([]);
    const articlesToFavorites = ref([]);
    const favoriteIndexes = ref([]);
    const baseUrl = 'https://api.openalex.org/'
    const searchUrl = ref('');

    // Here are the examples for searching found on https://api.openalex.org
    // https://api.openalex.org/works?filter=publication_date:2020-02-01 -- by publication date
    // https://api.openalex.org/authors?filter=display_name.search:john%20smith -- by author
    // https://api.openalex.org/works -- all works

    const getArtiles = () => {
        // most recent for 2024 https://api.openalex.org/works?filter=publication_year:2024
        if (searchUrl.value === '') searchUrl.value = 'works?filter=publication_year:2024'
        axios.get(baseUrl + searchUrl.value)
        .then((response) => {
            articles.value = response.data.results;
        });
    }

    onMounted(() => {
        getArtiles();
        getFavoriteArticles();

    })

    const addToFavorites = (a, i) => {
        a["added"] = true;
        articlesToFavorites.value.push(a);
        localStorage.setItem("articles", JSON.stringify(articlesToFavorites.value));
        favoriteIndexes.value.push(a.id)
    }

    const getFavoriteArticles = () => {
        const favouriteArticles = JSON.parse(localStorage.getItem("articles"));
        if (favouriteArticles) {
            articlesToFavorites.value = favouriteArticles;
        }
    };


    const searchTerm = (s) => {
        if (s.value === '') return;
        if (s.searchBy === 'author') { searchUrl.value = 'authors?filter=display_name.search:' + s.value; }
        if (s.searchBy === 'date') { searchUrl.value = 'works?filter=publication_date:' + s.value; }
        if (s.searchBy === 'general') { searchUrl.value = 'works?search=' + s.value; }
        
        axios.get(baseUrl + searchUrl.value)
        .then((response) => {
            articles.value = response.data.results;
        });

    }

</script>

<template>
    <div>
        <h1>Articles</h1>
        <SearchBar @search-term="searchTerm"  class="mt-4 mb-8"/>

        <p>List of most recent artcles</p>
        <ul v-for="(article, index) in articles" :key=index>
            <ArticleCard :article="article"
             @add-to-favorites="addToFavorites(article, index)" />
        </ul>
    </div>
</template>

<style scoped>

</style>