<template>
  <div class="cityButton" @click="toggleStatus(city.id)">
    {{ city.name }}
  </div>
</template>
    
<script>
export default {
  props: {
    city: {
      type: Object,
      required: true
    }
  },
  methods: {
    toggleStatus(id) {
      this.$emit('togglestatus', id)
    }
  }
}
</script>
    
<style scoped>
.cityButton {
  color: teal;
  border: 2px solid teal;
  border-radius: 10px;
  padding: 10px 15px;
  margin: 0 10px;
  cursor: pointer;
  transition: 0.3s;
}

.cityButton:hover {
  background-color: teal;
  color: white;
}
</style>