<template>
<!--    // check if task is done and put it in colpetd tasks list-->
    <li v-if="task.done" v-on:click="taskClicked" class="checked">{{task.title}}
        <span v-on:click="editButton" class="edit">⁖</span>
        <span v-on:click="closeButton" class="close">×</span>
    </li>
<!--   // else task is in todo list-->
    <li v-else v-on:click="taskClicked">{{task.title}}
        <span v-on:click="editButton" class="edit">⁖</span>
        <span v-on:click="closeButton" class="close">×</span>
    </li>
</template>

<script>
    export default {
        name: "ListItem",
        props: {
            task: Object
        },
        methods: {
            editButton() {
                console.log('editClick event from ListItem comonent')
                this.$emit('editClick', this.task);
            },
            closeButton() {
                console.log('closeClick event from ListItem comonent')
                this.$emit('closeClick', this.task);
            },
            taskClicked() {
                console.log('taskClicked event from ListItem comonent')
                this.$emit('taskClicked', this.task);
            },
        },

    }
</script>

<style scoped>
    /* Style the list items */
    ul li {
        cursor: pointer;
        position: relative;
        padding: 12px 8px 12px 40px;
        background: #d5d8dc;
        font-size: 18px;
        transition: 0.2s;
        list-style: none;

        /* make the list items unselectable */
        -webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
    }

    /* Set all odd list items to a different color (zebra-stripes) */
    ul li:nth-child(odd) {
        background: #eaecee;
    }

    /* Darker background-color on hover */
    ul li:hover {
        background: #d4efdf;
    }

    ul li:hover::before {
        content: '';
        position: absolute;
        border-color: green;
        border-style: solid;
        border-width: 0 4px 4px 0;
        top: 10px;
        left: 16px;
        transform: rotate(45deg);
        height: 15px;
        width: 7px;
        opacity: 0.25;
    }

    /* When clicked on, add a background color and strike out text */
    ul li.checked {
        background: #a6acaf;
        color: #fff;
        text-decoration: line-through;
    }

    ul li.checked:hover {
        background: #808b96;
    }

    /* Add a "checked" mark when clicked on */
    ul li.checked::before {
        content: '';
        position: absolute;
        border-color: green;
        border-style: solid;
        border-width: 0 4px 4px 0;
        top: 10px;
        left: 16px;
        transform: rotate(45deg);
        height: 15px;
        width: 7px;
    }

    /* Style the close button */
    .close {
        color: #e74c3c;
        font-weight: 700;
        position: absolute;
        right: 0;
        top: 0;
        padding: 12px 16px 12px 16px;
    }

    .close:hover {
        background-color: #e74c3c;
        color: white;
    }

    /* Style the close button */
    .edit {
        color: #2980b9;
        font-weight: 700;
        position: absolute;
        right: 44px;
        top: 0;
        padding: 12px 16px 12px 16px;
    }

    .edit:hover {
        background-color: #2980b9;
        color: white;
    }
</style>