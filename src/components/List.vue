<template>
    <ul>
        <li v-bind:key="item" v-for="item in list">
            <span>{{item}}</span>
        </li>
    </ul>
</template>

<script>
    export default {
        name: "List",
        // props: ['list']
        props: {
            list: Array
        }
    }
</script>

<style scoped>

</style>