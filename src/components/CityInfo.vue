<template>
    <div class="table-line" @mouseover="hover = true" @mouseleave="hover = false" :class="{ hovered: hover }">
        <p @click="toggleStatus(city.id)" class="table-item">{{ city.name }} <strong class="remove"
                v-if="hover">x</strong></p>
        <p class="table-item">MinTemp</p>
        <p class="table-item">MaxTemp</p>
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
    data() {
        return {
            hover: false
        }
    },
    methods: {
        toggleStatus(id) {
            this.$emit('togglestatus', id)
        },
    }
}
</script>

<style scoped>
.table-line.hovered {
    background-color: teal;
    color: white;
}

.table-item {
    position: relative;
    width: 150px;
    text-align: center;
    font-size: 20px;
    padding: 10px;
}

.table-item:first-child {
    cursor: pointer;
}

.remove {
    position: absolute;
    right: 0;
    color: rgb(206, 206, 206);
    font-weight: 900;
    cursor: pointer;
}
</style>