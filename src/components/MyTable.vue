<template>
    <div v-if="!cities.length"> Вы ничего не выбрали</div>
    <div v-else-if="!isLoading" class="table">
        <div class="table-line first-line">
            <div v-for="(item, index) in title" :key="index" class="table-item">{{ item }}</div>
        </div>
        <city-info class="table-line" v-bind="$attrs" v-for="city in cities" :key="city.id" :city="city"
            :minTemp="fetchedTemperatures[city.id]?.temperature_2m_min[0]"
            :maxTemp="fetchedTemperatures[city.id]?.temperature_2m_max[0]" />
    </div>
    <div v-else>грузится</div>
</template>

<script>
import CityInfo from './CityInfo.vue';
import axios from 'axios';
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
            paramsToFetch: {
                "Kyiv": [50.45, 30.52],
                "Moscow": [55.75, 75],
                "London": [51.51, -0.13],
                "Berlin": [52.52, 13.41],
                "Paris": [48.85, 2.35]
            },
            fetchedTemperatures: [],
            isLoading: true,
        }
    },
    methods: {
        async fetchWeather() {
            this.isLoading = true;
            try {
                Promise.all(this.cities.map(city => {
                    const [lat, long] = this.paramsToFetch[city.name];
                    return axios
                        .get(`https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${long}&daily=temperature_2m_max,temperature_2m_min&timezone=Europe%2FMoscow`);
                })).then(responses => responses.map(el => el.data.daily)).then(data => {
                    this.fetchedTemperatures = data;
                    console.log(data);
                });
            } catch (e) {
                console.log(e)
            } finally {
                this.isLoading = false;
            }
        }
    },
    mounted() {
        this.fetchWeather()
    },
    watch: {
        cities() {
            this.fetchWeather();
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