<template>
  <div class="results">
    <div class="grid-container">
      <div v-for="(movie, index) in results" :key="index" class="grid-item">
        <div class="movie">
          <div class="image">
            <img :src="`http://image.tmdb.org/t/p/w500${movie.poster_path}`">
          </div>
          <div class="info">
            <div class="title">
              {{ movie.title }}
            </div>
            <div class="genres" v-for="(genre, index) in movie.genre_ids" :key="index">
              {{ genre }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['results'],
  name: 'Movie'
}
</script>
