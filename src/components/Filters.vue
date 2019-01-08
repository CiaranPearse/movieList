<template>
  <div class="results">
    <!-- ADD V FOR HERE -->
    <div v-for="(genre,index) in defaultGenres" :key="index" class="form-check form-check-inline">
      <input class="form-check-input" :id="genre.id" type="checkbox"  v-model="genre.checked" v-on:change="getfilteredData">
      <label class="form-check-label">
        {{ genre.name }} {{ genre.id }}
      </label>
    </div>
  </div>
</template>

<script>
export default {
  props: ['filters'],
  name: 'Filters',
  data () {
    return {
      defaultGenres: [],
      selectedGenres: [],
      ratingMin: 0,
      ratingMax: 10,
      lowestRating: 3
    }
  },
  mounted () {
    this.getfilteredData()
  },
  methods: {
    getfilteredData: function () {
      this.defaultGenres = this.filters
      this.selectedGenres = this.filters
      let filteredDataByfilters = []
      if (this.selectedFilters.length > 0) {
        filteredDataByfilters = this.filteredData.filter(obj => this.selectedFilters.every(id => obj.genre.indexOf(id) >= 0))
        this.selectedGenres = filteredDataByfilters
      }
    }
  },
  computed: {
    selectedFilters: function () {
      let filters = []
      let checkedFiters = this.defaultGenres.filter(obj => obj.checked)
      checkedFiters.forEach(element => {
        filters.push(element.id)
      })
      this.$emit('clicked', filters)
      return filters
    }
  }
}
</script>

<style scoped lang="scss">
.form-check {
	display: inline-block;
	width: 200px;
	margin: 4px;
	border: 1px solid gainsboro;
}
</style>
