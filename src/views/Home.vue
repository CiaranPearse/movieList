<template>
  <div class="home">
    <div v-if="loading === true" class="loading">
      <div class="fingerprint-spinner">
        <div class="spinner-ring"></div>
        <div class="spinner-ring"></div>
        <div class="spinner-ring"></div>
        <div class="spinner-ring"></div>
        <div class="spinner-ring"></div>
        <div class="spinner-ring"></div>
        <div class="spinner-ring"></div>
        <div class="spinner-ring"></div>
        <div class="spinner-ring"></div>
      </div>
      <h3>Loading</h3>
    </div>
    <div v-else>
    	<div v-if="error" class="showError">
        <p>{{ this.errorText }}</p>
      </div>
      <div v-else>
    	  <div v-if="movieError" class="showError">
          <p>{{ this.movieErrorText }}</p>
        </div>
        <Rating :filters="this.getAvailableFilters"  @ratingChanged="changeRating" />
        <Filters :filters="this.getAvailableFilters"  @clicked="reFilter" />
        <Movies :results="this.showFilteredMovies" :rating="this.rating" :allGenres="this.allGenres" />
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Rating from '@/components/Rating.vue'
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
      movieError: false,
      errorText: 'Opps! Something went wrong.',
      movieErrorText: 'Opps! Something went wrong.',
      allMovies: [],
      showMovies: [],
      allGenres: [],
      currentMoviesGenres: [],
      selectedGenres: [],
      rating: 3
    }
  },
  components: {
    Rating,
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
        .catch(error => {
        	this.loading = false
          this.error = true
          this.errorText = error.response.data.status_message
        })
    },
    getMovies () {
      const moviePath = 'https://api.themoviedb.org/3/movie/now_playing?api_key='
      const CORS_PROXY = 'https://cors-anywhere.herokuapp.com/'
      axios
        .get(CORS_PROXY + moviePath + this.apiKey)
        .then(response => {
          this.allMovies = response.data.results
          this.showMovies = response.data.results
          this.getIntiialMovies()
          this.loading = false
        })
        .catch(error => {
        	this.loading = false
          this.error = true
          this.errorText = error.response.data.status_message
        })
    },
    getIntiialMovies () {
      for (let thisMovie of this.allMovies) {
        for (let currentGenre of thisMovie.genre_ids) {
          if (this.currentMoviesGenres.indexOf(currentGenre) === -1) {
            this.currentMoviesGenres.push(currentGenre)
          }
        }
      }
    },
    reFilter: function (value) {
      this.selectedGenres = value
      this.movieError = false
      let movies = this.allMovies
      var theResult = movies.filter(x => x.genre_ids.some(g => value.includes(g)))
      this.showMovies = theResult
      if (theResult.length > 0) {
        return theResult
      } else {
        this.movieError = true
        this.movieErrorText = "No movies to Show"
      }
    },
    changeRating: function (value) {
      this.rating = value
      this.movieError = false
      let movies = this.allMovies
      var theResult = movies.filter(x => x.vote_average > value)
      this.showMovies = theResult
      if (theResult.length > 0) {
       return theResult
     } else {
        this.movieError = true
        this.movieErrorText = "No movies to Show"
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
    },
    showFilteredMovies () {
      return this.showMovies
    }
  }
}
</script>
