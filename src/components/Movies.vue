<template>
  <div class="results">
    <div class="grid-item" v-for="(movie, index) in showMovies" :key="index">
      <div class="movie">
        <div class="image">
          <img :src="`http://image.tmdb.org/t/p/w185${movie.poster_path}`">
        </div>
        <div class="info">
          <div class="title">
            {{movie.title}}
          </div>
          <div class="genres" v-for="(genre, index) in movie.genre_ids" :key="index">
            {{ genre }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['results'],
  name: 'Movie',
  data () {
    return {
      showMovies: []
    }
  },
  mounted () {
    this.showMovies = this.results
  }
}
</script>

<style scoped lang="scss">
.results {
  .grid-item {
  	border: 1px solid gainsboro;
    width: 22%;
    display: inline-block;
    margin: 4px;
    float: left;
    .movie {
      .image {

      }
      .info {
        .title {

        }
        .genre {
        	display: inline-block;
        	padding: 4px;
        }
      }
    }
   }
}
</style>