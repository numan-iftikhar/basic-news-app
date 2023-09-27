<template>
    <q-layout>
      <q-header>
        <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.8.1/css/all.css">

<div class="wrapper">
  <div class="navbar">
    <div class="menu">
      Logo
    </div>
    <div class="searchbar">
      <input type="text" placeholder="search" v-model="searchInput">
      <div class="icon">
        <i class="fas fa-search" @click="toggleActiveClass"></i>
      </div>
    </div>
  </div>
</div>
      </q-header>

      <div class="container">
  <main class="grid" v-if="!isDetail">
    <article v-for="article, index in newsData" :key="index">
      <img :src="article.urlToImage" :alt="article.title">
      <div class="text">
        <h6>{{ article.title }}</h6>
        <p>{{ article.description }}</p>
        <button class="news-btn" @click="readMore(article)">Read More</button>
      </div>
    </article>
  </main>

  <div class="news-detail" v-if="isDetail">
    <img :src="detailArticle.urlToImage" alt="News Image" />
    <h6>{{ detailArticle.title }}</h6>
    <p>{{ detailArticle.content }}</p>
  </div>
</div>
    </q-layout>
  </template>

  <script setup>
import axios from "axios";
import { ref, onMounted, computed, watch } from "vue";
let searchInput = ref('');
const isDetail = ref(false)
const detailArticle = ref('')
const newsData = ref([])


const apiKey = computed(() => {
  return process.env.VUE_APP_API_KEY;
})

function toggleActiveClass() {
  const iconElement = document.querySelector(".icon");
  const textInputElement = document.querySelector("input[type='text']");

  iconElement.classList.toggle("active");
  textInputElement.classList.toggle("active");
}

function readMore(article) {
  detailArticle.value = article;
  isDetail.value = true;
}

function getNewsListing(searchQuery){
    const apiKey = "a345db94fba14e5eb960c6908ebb28fa"
    axios.get(`https://newsapi.org/v2/everything?q=${searchQuery}&apiKey=${apiKey}`)
        .then(response => {
          newsData.value = response.data.articles;
          console.log(response)
        })
        .catch(error => {
          console.error('Error fetching data:', error);
        });
}

watch(searchInput, async (newVal) => {
    try {
      setTimeout(() => {
        getNewsListing(newVal)
      }, 1000);
    } catch (error) {
      console.log(error.message)
    }
})

onMounted(() => getNewsListing())

</script>
<style scoped>
@import url('https://fonts.googleapis.com/css?family=Josefin+Sans');


*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  text-decoration: none;
  outline: none;
  list-style: none;
  font-family: 'Josefin Sans', sans-serif;
}
.news-detail{
  padding: 10rem;
}
.news-detail img{
  height: 50vh;
  width: 100%;
}
.wrapper{
  width: 100%;
}

.wrapper .navbar{
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 0 30px;
  background: #2376aa;
  height: 60px;
}

.wrapper .navbar .menu ul li{
  display: inline-block;
  margin: 0 10px;
}

.wrapper .navbar .menu ul li a{
  color: #fff;
  text-transform: uppercase;
  letter-spacing: 3px;
  font-size: 12px;
}

.searchbar{
  position: relative;
}

.searchbar input[type="text"]{
  border: 0;
  padding: 0;
  width: 0px;
  height: 35px;
  border-radius: 3px;
  transition: all 0.3s ease;
}

.searchbar .icon{
  position: absolute;
  top: 0;
  right: 0;
  width: 35px;
  height: 100%;
  background: none;
  border-radius: 3px;
  color: #fff;
  transition: all 0.5s 0.3s ease;
}

.searchbar .icon i{
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
  cursor: pointer;
}

.banner img{
  width: 100%;
  height: 350px;
}

.searchbar .icon.active{
  background: #062333;
  transition: all 0.3s ease;
}

.searchbar input[type="text"].active{
  width: 250px;
  padding: 0 10px;
  transition: all 0.5s 0.2s ease;
}

@media screen and (max-width: 730px){
  .wrapper .navbar{
    padding: 0px;
    flex-direction: column;
    height: 200px;
    justify-content: center;
  }
  .wrapper .navbar .menu ul li{
    display: block;
    text-align: center;
    margin: 10px 0;
  }
  .searchbar .icon{
    margin-right: -20px;
  }
}
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
body {
  padding: 40px 0;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  grid-gap: 20px;
  align-items: stretch;
}

.grid > article {
  border: 1px solid #ccc;
  box-shadow: 2px 2px 6px 0px rgba(0, 0, 0, 0.3);
}

.grid > article img {
  max-width: 100%;
}

.grid .text {
  padding: 20px;
}
.news-btn{
  display: block;
    width: 100%;
    padding: 5px;
    color: white;
    background: #2376aa;
    border: none;
    border-radius: 5px;
    font-weight: 600;
}
</style>