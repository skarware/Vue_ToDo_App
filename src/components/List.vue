<template>
    <ul>
        <ListItem
                v-bind:key="task.id"
                v-bind:task="task"
                v-for="task in list"
                @editClick="$emit('editClick', task)"
                @closeClick="$emit('closeClick', task)"
                @taskClicked="$emit('taskClicked', task)"
        ></ListItem>
    </ul>
</template>

<script>
    import ListItem from './ListItem.vue'

    export default {
        name: "List",
        // props: ['list']
        props: {
            list: Array
        },
        components: {
            ListItem,
        },
    }
</script>

<style scoped>
    /* Remove margins and padding from the list */
    ul {
        margin: 0;
        padding: 0;
    }
</style>