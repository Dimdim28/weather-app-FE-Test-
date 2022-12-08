<template>
  <div class="app-wrapper">
    <cities-list :cities="cities.filter(obj => !obj.active)" @togglestatus="toggleActivityStatus" />
    <my-table :cities="cities.filter(obj => obj.active)" @togglestatus="toggleActivityStatus" />
  </div>
</template>

<script>
import MyTable from '@/components/MyTable.vue'
import CitiesList from '@/components/CitiesList.vue'
export default {
  name: 'App',
  components: {
    MyTable, CitiesList
  },
  data() {
    return {
      cities: [
        { id: 0, name: 'Kyiv', active: false },
        { id: 1, name: 'Moscow', active: false },
        { id: 2, name: 'London', active: false },
        { id: 3, name: 'Berlin', active: false },
        { id: 4, name: 'Paris', active: false }
      ]
    }
  },
  methods: {
    toggleActivityStatus(id) {
      const city = this.cities[id]
      city.active = !city.active
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 100%;
  height: 100vh;
}

.app-wrapper {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
</style>
