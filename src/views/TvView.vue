<script setup>
import { ref, onMounted } from 'vue'
import api from '@/plugins/axios'
const tvshow = ref([])

const listTV = async (with_genres) => {
  const response = await api.get('discover/tv', {
    params: {
      with_genres: with_genres,
      language: 'pt-BR',
    },
  })
  tvshow.value = response.data.results
}

const genres = ref([])

onMounted(async () => {
  const response = await api.get('genre/tv/list?language=pt-BR')
  genres.value = response.data.genres
})
</script>

<template>
  <h1>Programas de TV</h1>
  <ul class="genre-list">
    <li v-for="genre in genres" :key="genre.id" @click="listTV(genre.id)" class="genre-item">
      {{ genre.name }}
    </li>
  </ul>
  <div class="tv-list">
    <div v-for="tvshows in tvshow" :key="tvshows.id" class="tvshows-card">
      <img :src="`https://image.tmdb.org/t/p/w500${tvshows.poster_path}`" :alt="tvshows.title" />
      <div class="tvshows-details">
        <p class="tvshows-title">{{ tvshows.name }}</p>
        <p class="tvshows-release-date">{{ tvshows.first_air_date }}</p>
        <p class="tvshows-genres">{{ tvshows.with_genres }}</p>
        <p class="tvshows-original-name">{{ tvshows.original_name }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.genre-list {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 2rem;
  list-style: none;
  margin-bottom: 2rem;
}

.genre-item {
  background-color: #7d8a2e;
  border-radius: 1rem;
  padding: 0.5rem 1rem;
  color: #fff;
}

.genre-item:hover {
  cursor: pointer;
  background-color: #7d8a2e;
  box-shadow: 0 0 0.5rem #5d6424;
}

.tv-list {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.tvshows-card {
  width: 15rem;
  height: 30rem;
  border-radius: 0.5rem;
  overflow: hidden;
  box-shadow: 0 0 0.5rem #000;
}

.tvshows-card img {
  width: 100%;
  height: 20rem;
  border-radius: 0.5rem;
  box-shadow: 0 0 0.5rem #000;
}

.tvshows-details {
  padding: 0 0.5rem;
}

.tvshows-title {
  font-size: 1.1rem;
  font-weight: bold;
  line-height: 1.3rem;
  height: 3.2rem;
}
.tvshows-genres {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: flex-start;
  justify-content: center;
  gap: 0.2rem;
}

.tvshows-genres span {
  background-color: #748708;
  border-radius: 0.5rem;
  padding: 0.2rem 0.5rem;
  color: #fff;
  font-size: 0.8rem;
  font-weight: bold;
}

.tvshows-genres span:hover {
  cursor: pointer;
  background-color: #455a08;
  box-shadow: 0 0 0.5rem #748708;
}
</style>
