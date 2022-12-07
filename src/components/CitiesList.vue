<template>
  <div v-if="cities.length" class="citiesList">
    <city-button v-bind="$attrs" v-for="city in cities" :city="city" :key="city.id" />
  </div>
</template>
    
<script>
import CityButton from '@/components/CityButton.vue'
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
  border: 2px solid teal;
  display: flex;
  justify-content: center;
  padding: 5px 10px;
  font-size: 20px;
  font-weight: 600;
}
</style>