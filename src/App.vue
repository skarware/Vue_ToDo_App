<template>
    <div id="app">
        <div class="container">

            <header class="flex-wrapper">
                <h1>{{appTitle}}</h1>
                <svg class="svg-inline--fa fa-check-double fa-w-16" aria-hidden="true" focusable="false"
                     data-prefix="fas" data-icon="check-double" role="img" xmlns="http://www.w3.org/2000/svg"
                     viewBox="0 0 512 512" data-fa-i2svg="">
                    <path fill="currentColor"
                          d="M505 174.8l-39.6-39.6c-9.4-9.4-24.6-9.4-33.9 0L192 374.7 80.6 263.2c-9.4-9.4-24.6-9.4-33.9 0L7 302.9c-9.4 9.4-9.4 24.6 0 34L175 505c9.4 9.4 24.6 9.4 33.9 0l296-296.2c9.4-9.5 9.4-24.7.1-34zm-324.3 106c6.2 6.3 16.4 6.3 22.6 0l208-208.2c6.2-6.3 6.2-16.4 0-22.6L366.1 4.7c-6.2-6.3-16.4-6.3-22.6 0L192 156.2l-55.4-55.5c-6.2-6.3-16.4-6.3-22.6 0L68.7 146c-6.2 6.3-6.2 16.4 0 22.6l112 112.2z"></path>
                </svg>
            </header>

            <div id="addTask" class="flex-wrapper">
                <Input v-bind:inputStr="inputStr"
                       @onInput="addTask"
                       @updateInput="updateTask"
                />
            </div>

            <h2 v-if="tasksArr.length === 0">Task list is empty</h2>

            <div id="tasks-container">
                <div v-if="isTodoListNotEmpty" id="toDoTasks">
                    <svg aria-hidden="true" focusable="false" data-prefix="fas" data-icon="clipboard-list" role="img"
                         xmlns="http://www.w3.org/2000/svg" viewBox="0 0 384 512" data-fa-i2svg=""
                         class="svg-inline--fa fa-clipboard-list fa-w-12">
                        <path fill="currentColor"
                              d="M336 64h-80c0-35.3-28.7-64-64-64s-64 28.7-64 64H48C21.5 64 0 85.5 0 112v352c0 26.5 21.5 48 48 48h288c26.5 0 48-21.5 48-48V112c0-26.5-21.5-48-48-48zM96 424c-13.3 0-24-10.7-24-24s10.7-24 24-24 24 10.7 24 24-10.7 24-24 24zm0-96c-13.3 0-24-10.7-24-24s10.7-24 24-24 24 10.7 24 24-10.7 24-24 24zm0-96c-13.3 0-24-10.7-24-24s10.7-24 24-24 24 10.7 24 24-10.7 24-24 24zm96-192c13.3 0 24 10.7 24 24s-10.7 24-24 24-24-10.7-24-24 10.7-24 24-24zm128 368c0 4.4-3.6 8-8 8H168c-4.4 0-8-3.6-8-8v-16c0-4.4 3.6-8 8-8h144c4.4 0 8 3.6 8 8v16zm0-96c0 4.4-3.6 8-8 8H168c-4.4 0-8-3.6-8-8v-16c0-4.4 3.6-8 8-8h144c4.4 0 8 3.6 8 8v16zm0-96c0 4.4-3.6 8-8 8H168c-4.4 0-8-3.6-8-8v-16c0-4.4 3.6-8 8-8h144c4.4 0 8 3.6 8 8v16z"></path>
                    </svg>
                    <h2> Tasks To Do</h2>
                    <List v-bind:list="todoListComputed"
                          @editClick="editClick"
                          @closeClick="closeClick"
                          @taskClicked="taskClicked"
                    ></List>
                </div>
                <div v-if="isCompletedListNotEmpty" id="completedTasks">
                    <svg class="svg-inline--fa fa-clipboard-check fa-w-12" aria-hidden="true" focusable="false"
                         data-prefix="fas" data-icon="clipboard-check" role="img" xmlns="http://www.w3.org/2000/svg"
                         viewBox="0 0 384 512" data-fa-i2svg="">
                        <path fill="currentColor"
                              d="M336 64h-80c0-35.3-28.7-64-64-64s-64 28.7-64 64H48C21.5 64 0 85.5 0 112v352c0 26.5 21.5 48 48 48h288c26.5 0 48-21.5 48-48V112c0-26.5-21.5-48-48-48zM192 40c13.3 0 24 10.7 24 24s-10.7 24-24 24-24-10.7-24-24 10.7-24 24-24zm121.2 231.8l-143 141.8c-4.7 4.7-12.3 4.6-17-.1l-82.6-83.3c-4.7-4.7-4.6-12.3.1-17L99.1 285c4.7-4.7 12.3-4.6 17 .1l46 46.4 106-105.2c4.7-4.7 12.3-4.6 17 .1l28.2 28.4c4.7 4.8 4.6 12.3-.1 17z"></path>
                    </svg>
                    <h2> Completed Tasks</h2>
                    <List v-bind:list="completedListComputed"
                          @editClick="editClick"
                          @closeClick="closeClick"
                          @taskClicked="taskClicked"
                    ></List>
                </div>
            </div>
        </div>

        <button v-on:click="clearTasksArr" id="clearButton" class="pure-button">Clear All Tasks</button>

    </div>
</template>

<script>
    import List from './components/List.vue';
    import Input from './components/Input.vue';

    export default {
        name: 'App',
        data() {
            return {
                appTitle: 'ToDo::List ',
                name: '',
                inputStr: '',
                indexToEditTasksArr: null,
                tasksArr: []
            }
        },
        watch: {
            // auto save tasks array to localStorage
            tasksArr: {
                // This will let Vue know to look inside the array
                deep: true,
                // We have to move our method to a handler field
                handler() {
                    console.log("watch:tasksArr: Auto save invoked");
                    this.saveTasksArr();
                }
            },
        },
        computed: {
            todoListComputed: function () {
                return this.tasksArr.filter(task => !task.done);
            },
            completedListComputed: function () {
                return this.tasksArr.filter(task => task.done);
            },
            // increase Id number and return for new obj is pushed into array
            nextId() {
                if (this.tasksArr.length > 0) {
                    return this.tasksArr[this.tasksArr.length - 1].id + 1;
                }
                return 0;
            },
            isTodoListNotEmpty() {
                return this.todoListComputed.length > 0;
            },
            isCompletedListNotEmpty() {
                return this.completedListComputed.length > 0;
            },
        },
        mounted() {
            this.init();
        },
        methods: {
            init() {
                console.log("init() method invoked")
                // load tasksArray from localStorage if it's not empty
                if (localStorage.getItem('tasks')) {
                    this.tasksArr = JSON.parse(localStorage.getItem('tasks'));
                    console.log("init() method invoked: loading data from localStorage")
                }
                // save tasks array for app to function properly
                this.saveTasksArr();
            },
            saveTasksArr() {
                // save tasksArr into localStorage
                console.log("saveTasksArr() method invoked: saving tasksArr into localStorage");
                localStorage.setItem('tasks', JSON.stringify(this.tasksArr));
            },
            clearTasksArr() {
                console.info("clearTasksArr() method invoked: clearing tasksArr and localStorage");
                // clear tasks from localStorage
                localStorage.clear();
                // reset tasks array to empty one
                this.tasksArr = [];
            },
            addTask(taskTitle) {
                if (taskTitle !== '') {
                    // push new task obj into array
                    this.tasksArr.push(
                        {
                            id: this.nextId,
                            title: taskTitle,
                            done: false
                        }
                    );
                }
            },
            updateTask(taskTitle) {
                // if empty string received reset inputStr to empty string so Input component knows the editing of task is done
                if (taskTitle === '') {
                    this.inputStr = '';
                }
                // else update title in array by its index
                else {
                    this.tasksArr[this.indexToEditTasksArr].title = taskTitle;
                }
                console.log(taskTitle);
            },
            editClick(task) {
                console.log("editClick eventas is App componento")
                console.log(task);
                this.inputStr = task.title;
                // find task index in tasksArr for live update edit
                this.tasksArr.forEach((el, idx) => {
                    if (el.id.toString() === task.id.toString()) {
                        this.indexToEditTasksArr = idx;
                    }
                });
            },
            closeClick(task) {
                console.log("closeClick eventas is App componento")
                console.log(task);
                console.log(this.tasksArr);
                // delete clicked task from array
                this.tasksArr.forEach((el, idx) => {
                    if (el.id.toString() === task.id.toString()) {
                        this.tasksArr.splice(idx, 1);
                    }
                });
                console.log(this.tasksArr);
            },
            taskClicked(task) {
                console.log("taskClicked eventas is App componento")
                console.log(task);
                // Check clicked task in array
                this.tasksArr.forEach((el, idx) => {
                    if (el.id.toString() === task.id.toString()) {
                        this.tasksArr[idx].done = !task.done;
                    }
                });
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
        /*margin-top: 60px;*/
    }

    /* Include the padding and border in an element's total width and height */
    * {
        box-sizing: border-box;
    }

    body {
        background-color: #f8f9f9;
        text-align: center;
    }

    h1, h2 {
        color: #566573;
        padding: 25px 0;
        margin: 0;
        display: inline-block;
        /*vertical-align: middle;*/
    }

    .fa-check-double {
        color: green;
        height: 2em;
        margin: 0 10px;
    }

    .fa-clipboard-check {
        height: 2em;
        margin: 0 10px;
    }

    .fa-clipboard-list {
        height: 2em;
        margin: 0 10px;
        text-align: center;

    }

    .container {
        max-width: 600px;
        width: 80%;
        margin: 0 auto;
    }

    .flex-wrapper {
        display: flex;
        justify-content: center;
        align-items: center;
    }

    #tasks-container {
    }

    #tasks-container ul {
        text-align: left;
    }

    #clearButton {
        /*background: rgb(202, 60, 60); !* this is a maroon *!*/
        color: white;
        background: #cb4335;
        margin-top: 50px;
    }

    #clearButton:hover {
        transform: scale(0.95);
    }


</style>
