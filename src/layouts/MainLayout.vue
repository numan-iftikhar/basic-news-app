<template>
    <q-layout>
      <q-header>
        <!-- Navbar content -->
        <div class="row items-center q-pa-sm">
          <!-- Branch name -->
          <q-toolbar-title>News App</q-toolbar-title>
          
          <!-- Search bar -->
          <q-input v-model="search" filled type="search" hint="Search">
        <template v-slot:append>
          <q-icon name="search" />
        </template>
      </q-input>
        </div>
      </q-header>
  
      <div class="news-wrapper">
        <div class="card" v-for="article, index in newsData" :key="index">
            <div class="card__img">
                <img :src="article.urlToImage" alt="Card Image">
            </div>
            <div class="card__title">{{ article.title }}</div>
            <div class="card__desc">{{ article.description }}</div>
        </div>
      </div>
    </q-layout>
  </template>

  <script setup>
import axios from "axios";
import { ref, onMounted, computed } from "vue";
let searchInput = ref('');
const newsData = ref([])


const apiKey = computed(() => {
  return process.env.VUE_APP_API_KEY;
})

function getNewsListing(){
    const apiKey = "f3ae35dedebf4e3f9341ae2ab46c878b"
    axios.get(`https://newsapi.org/v2/top-headlines?sources=techcrunch&apiKey=${apiKey}`)
        .then(response => {
          newsData.value = response.data.articles;
          console.log(response)
        })
        .catch(error => {
          console.error('Error fetching data:', error);
        });
}

onMounted(() => getNewsListing())

</script>
<style scoped>
.news-wrapper{
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
}
.card{
    flex: 1;
    min-width: 25%;
}
.card .card__img > img{
    height: 100%;
    width: 100%;
    object-fit: cover;
}
</style>