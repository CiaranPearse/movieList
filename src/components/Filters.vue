<template>
  <div class="filters">
    <div v-for="(genre,index) in defaultGenres" :key="index" class="checkItem">
      <label class="checkContainer">{{ genre.name }}
      <input type="checkbox" :id="genre.id" v-model="genre.checked" v-on:change="getfilteredData">
      <span class="checkmark"></span>
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
      filteredData: [],
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

</style>
