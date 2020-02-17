<template>
    <form v-on:submit.prevent
          class="pure-form"
    >
        <svg class="svg-inline--fa fa-plus-square fa-w-14" aria-hidden="true" focusable="false" data-prefix="far"
             data-icon="plus-square" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"
             data-fa-i2svg="">
            <path fill="currentColor"
                  d="M352 240v32c0 6.6-5.4 12-12 12h-88v88c0 6.6-5.4 12-12 12h-32c-6.6 0-12-5.4-12-12v-88h-88c-6.6 0-12-5.4-12-12v-32c0-6.6 5.4-12 12-12h88v-88c0-6.6 5.4-12 12-12h32c6.6 0 12 5.4 12 12v88h88c6.6 0 12 5.4 12 12zm96-160v352c0 26.5-21.5 48-48 48H48c-26.5 0-48-21.5-48-48V80c0-26.5 21.5-48 48-48h352c26.5 0 48 21.5 48 48zm-48 346V86c0-3.3-2.7-6-6-6H54c-3.3 0-6 2.7-6 6v340c0 3.3 2.7 6 6 6h340c3.3 0 6-2.7 6-6z"></path>
        </svg>
        <input v-if="inputStr === ''"
               v-on:keyup.enter="getInput"
               v-model="taskTitle" id="newTask"
               type="text"
               placeholder="Add New Task"
               required="required"
               ref="focusInput"
        />
        <input v-else
               v-on:keyup="updateInput"
               v-on:keyup.enter="finishUpdateInput"
               v-model="taskTitle" id="updateTask"
               type="text"
               required="required"
               ref="focusInput"
        />
    </form>
</template>

<script>
    export default {
        name: "Input",
        data() {
            return {
                taskTitle: '',
            }
        },
        props: {
            inputStr: String
        },
        watch: {
            // watch then inputStr changes and task edit begins
            inputStr(newVal) {
                // change input value with new value from App component
                this.taskTitle = newVal;
                // focus Update input
                this.setFocus();
            }
        },
        methods: {
            setFocus() {
                this.$refs.focusInput.focus();
            },
            getInput() {
                console.log('getInput method in Input component runs');
                // on enter emit task from the input
                this.$emit('onInput', this.taskTitle);
                // and reset task back to empty string for palceholder attribute to take place
                this.taskTitle = '';
            },
            updateInput() {
                console.log('updateInput method in Input component runs');
                // emit every keypress to get live update
                this.$emit('updateInput', this.taskTitle);
            },
            finishUpdateInput() {
                console.log('finishUpdateInput method in Input component runs');
                // send empty string if enter is pressed to finish task editing
                this.$emit('updateInput', '');
            }
        }
    }
</script>

<style scoped>

    .fa-plus-square {
        /*font-size: 2em;*/
        height: 2em;
        /*color: green;*/
        margin: 10px;
        text-align: center;
        vertical-align: middle;
    }

    #addTask form {
        display: flex;
        width: 100%;
    }

    #addTask input {
        padding: 10px 25px;
        width: 100%;
        background-color: #fef9e7;
        /*margin-right: 48px;*/
    }

    #tasks-container input {
        padding: 10px 25px;
        width: 100%;
        background-color: #ebf5fb;
        margin: 1px 0;
    }
</style>