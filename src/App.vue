<template>
    <div id="app">

        <h1>{{message}}</h1>
        <List v-bind:list="list"></List>
        <Input @onInput="addName"/>

    </div>
</template>

<script>
    import List from './components/List.vue'
    import Input from './components/Input.vue'

    export default {
        name: 'App',
        data() {
            return {
                message: 'Vue To:Do::App',
                name: '',
                list: [
                    "Jonas",
                    "Martynas"
                ]
            }
        },
        methods: {
            addName(value) {
                if (value !== '') {
                    this.list.push(value);
                }
            }
        },
        components: {
            List,
            Input,
        }
    }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
