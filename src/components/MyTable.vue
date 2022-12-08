<template>
    <div v-if="!cities.length"> Вы ничего не выбрали</div>
    <div v-else-if="!isWeatherLoading" class="table">
        <div class="table-line first-line">
            <div v-for="(item, index) in title" :key="index" class="table-item">{{ item }}</div>
        </div>
        <city-info class="table-line" v-bind="$attrs" v-for="city in cityData" :key="city.id" :name="city.name"
            :id="city.id" :minTemp="city.min" :maxTemp="city.max" />
    </div>
    <div v-else>грузится</div>
    <ul class="days-list">
        <li @click="firstDay"> first </li>
        <li @click="prevDay"> prev </li>
        <li>{{ сurrentDay }}</li>
        <li @click="nextDay"> next </li>
        <li @click="lastDay"> last </li>

    </ul>
    <sorting-bar class="sortWrapper" :options="sortOptions" v-model="selectedSort" />
</template>

<script>
import CityInfo from './CityInfo.vue';
import SortingBar from './SortingBar.vue';
import axios from 'axios';
export default {
    components: {
        CityInfo, SortingBar
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
            maxTemps: [],
            minTemps: [],
            isWeatherLoading: false,
            cityData: [],
            сurrentDay: 0,
            selectedSort: '',
            sortOptions: [{ value: 'min', name: 'minTemp' }, { value: 'max', name: 'maxTemp' }]
        }
    },
    methods: {
        async fetchWeather() {
            this.isWeatherLoading = true;
            try {
                Promise.all(this.cities.map(city => {
                    const [lat, long] = this.paramsToFetch[city.name];
                    return axios
                        .get(`https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${long}&daily=temperature_2m_max,temperature_2m_min&timezone=Europe%2FMoscow`);
                })).then(responses => responses.map(el => el.data.daily)).then(data => {
                    const min = data.map((city) => city.temperature_2m_min[this.сurrentDay]);
                    const max = data.map((city) => city.temperature_2m_max[this.сurrentDay]);
                    this.maxTemps = max;
                    this.minTemps = min
                }).then(() => this.setData())
            } catch (e) {
                console.log(e)
            } finally {
                this.isWeatherLoading = false;
            }
        },
        setData() {
            this.cityData = this.cities.map(el => {
                const { id, name } = el;
                const max = this.maxTemps[id];
                const min = this.minTemps[id];
                return { id, name, max, min };
            })
        },
        nextDay() {
            if (this.сurrentDay < 6) {
                this.сurrentDay++;
                this.fetchWeather();
            }
        },
        prevDay() {
            if (this.сurrentDay > 0) {
                this.сurrentDay--;
                this.fetchWeather();
            }
        },
        lastDay() {
            if (this.сurrentDay !== 6) {
                this.сurrentDay = 6;
                this.fetchWeather();
            }
        },
        firstDay() {
            if (this.сurrentDay != 0) {
                this.сurrentDay = 0;
                this.fetchWeather();
            }
        },
        changeCurrentOption(event) {
            this.selectedSort = event.target.value;
        }
    },
    watch: {
        cities() {
            this.fetchWeather();
        },
        selectedSort(newValue) {
            this.cityData.sort((city1, city2) => {
                return city1[newValue] - city2[newValue];
            })
        }
    },

}
</script>

<style scoped>
.table {
    height: 100%;
    color: teal;

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

.days-list {
    display: flex;
    align-items: center;
    justify-content: center;
}

.days-list li {
    background-color: teal;
    color: white;
    padding: 10px;
    font-size: 20px;
    list-style: none;
    cursor: pointer;
}

.days-list li:nth-child(3) {
    border-right: 2px solid white;
    border-left: 2px solid white;
    cursor: grab;
}

.days-list li:first-child {
    border-right: 2px solid white;
}

.days-list li:last-child {
    border-left: 2px solid white;
}

.sortWrapper {
    position: absolute;
    right: 0;
}
</style>