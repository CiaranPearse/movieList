<template>
  <div class="results">
    <div class="grid-container">
      <div v-for="(movie, index) in orderedMovies" :key="index" class="grid-item">
        <div class="movie">
          <div class="image">
            <img :src="`http://image.tmdb.org/t/p/w500${movie.poster_path}`">
          </div>
          <div class="info">
            <div class="title">
              {{ movie.title }}
            </div>
            <div class="genres">
              <div class="genre" v-for="(genre, index) in movie.genre_ids" :key="index">
                <div v-for="(singleGenre, gindex) in allGenres" :key="gindex">
                  <div v-if="singleGenre.id === genre">
                    {{ singleGenre.name }}
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  props: ['results', 'allGenres'],
  name: 'Movie',
  computed: {
    orderedMovies: function () {
      return _.orderBy(this.results, 'popularity', ['desc'])
    }
  }
}
</script>
