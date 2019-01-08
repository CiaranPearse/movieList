<template>
  <div class="home">
    <div v-if="loading === true">
      <h3>Loading</h3>
    </div>
    <div v-else>
      <Rating :filters="this.getAvailableFilters"  @clicked="changeRating" />
      <Filters :filters="this.getAvailableFilters"  @clicked="reFilter" />
      <Movies :results="this.showFilteredMovies" :rating="this.rating" />
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
      errorText: 'Opps! Something went wrong.',
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
      let movies = this.allMovies
      this.selectedGenres = value
      var theResult = movies.filter(x => x.genre_ids.some(g => value.includes(g)))
      this.showMovies = theResult
      return theResult
    },
    changeRating: function (value) {
      this.rating = value
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
