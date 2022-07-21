<script setup>
import { ref, onMounted, computed } from 'vue'

const query = ref('');
const my_anime = ref([]);
const search_results = ref('[]');
const loading = ref(false);

const my_anime_asc = computed(() => {
  return my_anime.value.sort((a, b) => {
    return a.title.localeCompare(b.title)
  })

})

const searchAnime = () => {
  if (query.value != '') {
    loading.value = true;
  const url = `https://api.jikan.moe/v4/anime?q=${query.value}`
  fetch(url)
    .then(res => res.json())
    .then(data => {
      search_results.value = data.data
    }).then(e => { loading.value = false })}
}

const handleInput = (e) => {
  if (!e.target.value) {
    
    search_results.value = []
  }


}

const add_anime = (anime) => {
  my_anime.value.push({});

}
</script>

<template>
  <form @submit.prevent="searchAnime">
    <input type="text" placeholder="Search for an anime..." v-model="query" @input="handleInput">
    <button type="submit">Search</button>
  </form>
  <h1 v-if="loading.value">Loading...</h1>
  <div v-if="search_results.length>0">

    <div v-for="anime in search_results">
      <h3>{{anime?.title}}</h3>
      <img :src="anime?.images?.jpg?.image_url" alt="">
    </div>
  </div>
</template>

