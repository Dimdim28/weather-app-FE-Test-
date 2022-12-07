<template>
    <div class="table">
        <div class="table-line">
            <div v-for="(item, index) in title" :key="index" class="table-item">{{ item }}</div>
        </div>
        <div class="table-line" v-for="(city) in cities" :key="city.id">
            <p class="table-item">{{ city.name }} <strong @click="toggleStatus(city.id)">-</strong></p>
            <p class="table-item">min temp</p>
            <p class="table-item">max temp</p>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        cities: {
            type: Array,
            required: true
        }
    },
    data() {
        return {
            title: ['City', 'MinTemp', 'MaxTemp'],
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
.table {
    height: 100%;
    color: teal;
    border: 2px solid teal;
    border-top: none;
}

.table-line {
    display: flex;
    width: 100%;
    justify-content: space-around;
}

.table-item {
    width: 150px;
    text-align: center;
    font-size: 20px;
}
</style>