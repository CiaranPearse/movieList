<template>
  <div class="results">
    <!-- ADD V FOR HERE -->
    <div class="form-select">
    	<select v-model="lowestRating"  v-on:change="changeRating">
        <option disabled value="">Please select one</option>
        <option>0</option>
        <option>0.5</option>
        <option>1.0</option>
        <option>1.5</option>
        <option>2</option>
        <option>2.5</option>
        <option selected>3.0</option>
        <option>3.5</option>
        <option>4</option>
        <option>4.5</option>
        <option>5.0</option>
        <option>5.5</option>
        <option>6</option>
        <option>6.5</option>
        <option>7.0</option>
        <option>7.5</option>
        <option>8</option>
        <option>8.5</option>
        <option>9.0</option>
        <option>9.5</option>
        <option>10</option>
      </select>
    </div>
  </div>
</template>

<script>
export default {
  props: ['rating'],
  name: 'Rating',
  data () {
    return {
      lowestRating: 3
    }
  },
  methods: {
  },
  computed: {
    changeRating: function () {
      this.$emit('clicked', this.lowestRating)
      return this.lowestRating
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
