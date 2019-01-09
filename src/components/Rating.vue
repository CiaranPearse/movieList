<template>
  <div class="ratings">
    <div class="rating">
      Choose Min. Rating: 
      <select v-model="lowestRating"  v-on:change="changeRating">
        <option value="">Please select one</option>
        <option value="0">0</option>
        <option value="0.5">0.5</option>
        <option value="1">1</option>
        <option value="1.5">1.5</option>
        <option value="2">2</option>
        <option value="2.5">2.5</option>
        <option value="3" selected>3</option>
        <option value="3.5">3.5</option>
        <option value="4">4</option>
        <option value="4.5">4.5</option>
        <option value="5">5</option>
        <option value="5.5">5.5</option>
        <option value="6">6</option>
        <option value="6.5">6.5</option>
        <option value="7">7</option>
        <option value="7.5">7.5</option>
        <option value="8">8</option>
        <option value="8.5">8.5</option>
        <option value="9">9</option>
        <option value="9.5">9.5</option>
        <option value="10">10</option>
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
  computed: {
    changeRating: function () {
      var pointNum = parseFloat(this.lowestRating);
      this.$emit('ratingChanged', pointNum)
      return pointNum
    }
  }
}
</script>

<style scoped lang="scss">

</style>
