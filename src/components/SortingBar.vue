<template>
    <select :value="modelValue" class="sort" @change="(event) => $emit('update:modelValue', event.target.value)">
        <option disabled value="choose sort params"></option>
        <option v-for="option in options" :key="option.value" :value="option.value"> {{ option.name }} </option>
    </select>
</template>
      
<script>
export default {
    components: {
    },
    props: {
        modelValue: {
            type: String,
            required: false,
        },
        options: {
            type: Array,
            default: () => []
        }
    },
}
</script>
      
<style scoped>
.sort {
    color: teal;
    padding: 5px 10px;
    font-size: 20px;
    font-weight: 600;
}
</style>