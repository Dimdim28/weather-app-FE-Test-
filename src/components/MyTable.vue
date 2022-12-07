<template>
    <div class="table">
        <div class="table-line first-line">
            <div v-for="(item, index) in title" :key="index" class="table-item">{{ item }}</div>
        </div>
        <city-info class="table-line" v-bind="$attrs" v-for="city in cities" :key="city.id" :city="city" />
    </div>
</template>

<script>
import CityInfo from './CityInfo.vue';
export default {
    components: {
        CityInfo
    },
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

.table-line:first-child {
    margin: 10px 0;
}

.table-line {
    display: flex;
    width: 100%;
    justify-content: space-around;
}

.table-line.first-line {
    background-color: teal;
    color: white;
}

.table-item {
    width: 150px;
    text-align: center;
    font-size: 20px;
    position: relative;
    padding: 10px;
}
</style>