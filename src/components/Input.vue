<template>
    <input v-on:keyup.enter="getInput" v-model="value">
</template>

<script>
    export default {
        name: "Input",
        data() {
            return {
                value: '',

            }
        },
        methods: {
            getInput() {
                console.log('getInput metodas Input component works')
                this.$emit('onInput', this.value);
                this.value = '';
            }
        }
    }
</script>

<style scoped>

</style>