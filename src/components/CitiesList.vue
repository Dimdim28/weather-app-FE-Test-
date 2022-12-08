<template>
  <div class="citiesList">
    <p class="message" v-if="!cities.length"> You have choosed all cities from the list =)</p>
    <city-button v-bind="$attrs" v-for="city in cities" :city="city" :key="city.id" />
  </div>
</template>
    
<script>
import CityButton from './CityButton.vue'
export default {
  components: {
    CityButton
  },
  props: {
    cities: {
      type: Array,
      required: true
    }
  }
}
</script>
    
<style scoped>
.citiesList {
  color: teal;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 5px 10px;
  font-size: 20px;
  font-weight: 600;
  height: 61px;
}
</style>