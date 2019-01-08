<template>
  <div class="home">
  	<div v-if="loading === true">
  		<h3>Loading</h3>
  	</div>
  	<div v-else>
      <Filters :filters="this.getAvailableFilters" />
      <Movies :results="this.allMovies"/>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Filters from '@/components/Filters.vue'
import Movies from '@/components/Movies.vue'
import axios from 'axios'

export default {
  name: 'home',
  data () {
    return {
    	apiKey: 'e3108a98b5d30b80374472848ffe7f10',
      loading: true,
      error: false,
      errorText: 'Opps! Something went wrong.',
      allMovies: [],
      allGenres: [],
      currentMoviesGenres: []
    }
  },
  components: {
    Filters,
    Movies
  },
  mounted () {
  	this.getGenres()
  	this.getMovies()
  },
  methods: {
    getGenres () {
      const genrePath = 'https://api.themoviedb.org/3/genre/movie/list?api_key='
      const CORS_PROXY = 'https://cors-anywhere.herokuapp.com/'
      axios
        .get(CORS_PROXY + genrePath + this.apiKey)
        .then(response => {
          this.allGenres = response.data.genres
        })
    },
    getMovies () {
      const moviePath = 'https://api.themoviedb.org/3/movie/now_playing?api_key='
      const CORS_PROXY = 'https://cors-anywhere.herokuapp.com/'
      axios
        .get(CORS_PROXY + moviePath + this.apiKey)
        .then(response => {
          this.allMovies = response.data.results
          for (let thisMovie of response.data.results) {
            for (let currentGenre of thisMovie.genre_ids) {
              if (this.currentMoviesGenres.indexOf(currentGenre) === -1) {
                this.currentMoviesGenres.push(currentGenre)
              }
            }
          }
          this.loading = false
        })
    },
    populateMovies (value) {
      for (let thisMovie of value) {
        for (let currentGenre of thisMovie.genre_ids) {
          if (this.currentMoviesGenres.indexOf(currentGenre) === -1) {
            this.currentMoviesGenres.push(currentGenre)
          }
        }
      }
    }
  },
  computed: {
  	getAvailableFilters: function () {
      let findFilterNames = this.currentMoviesGenres
      let allTheGenres = this.allGenres
      let allFilters = []
      for (var i = 0, len = findFilterNames.length; i < len; i++) {
        var tester = allTheGenres.find(x => x.id === findFilterNames[i])
        allFilters.push({ id: findFilterNames[i], name: tester.name, checked: true })
      }
      return allFilters
    }
  }
}
</script>
