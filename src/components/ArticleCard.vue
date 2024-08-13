<script setup> 
import { reactive, ref } from 'vue'

const props = defineProps({
  article: {
    type: Object,
    default: () => {},
  },
});

const articleRef = ref([]);

const emit = defineEmits(["add-to-favorites"]);

const addToFavorites = () => {
     emit("add-to-favorites", articleRef.value);
}

</script>

<template>
    <div class="card card-article">
        <div> {{article.display_name}} </div>
        <div><a :href=article.id target="_blank"> {{article.id}} </a> </div>
        <div> {{article.publication_date}} </div>
        <br>
        <button v-show=!article.added @click="addToFavorites()" class="button-secondary">Add to Favorites</button>
    </div>
</template>

<style scoped>

  .card-article {
      margin: 16px 0;
  }

  a {
      text-decoration: underline;
  }
</style>