<template>
    <div id="app">
        <TodoList v-bind:user="user"></TodoList>
    </div>
</template>

<script>
    import axios from 'axios';

    import TodoList from "./components/TodoList";

    export default {
        name: 'app',
        components: {
            TodoList,
        },
        data() {
            return {
                new_todo: "",
                user: {
                    username: "soulevans",
                    password: "hidden",
                    todo_list: []
                }
            }
        },
        mounted() {
            this.refresh();
        },
        methods: {
            refresh: function () {
                this.user.todo_list = [];
                axios.get('http://127.0.0.1:3000/api/tasks')
                    .then(res => {
                        res.data.list.forEach(task => this.user.todo_list.push(task));
                    });
            }
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #2c3e50;
    }

    html, body {
        margin: 0;
        padding: 0;
    }

    body {
        margin: 10px;
        /*background: linear-gradient(45deg, #4ed8ff -100%, #5bff36);*/
    }

    .done {
        color: gray;
        text-decoration: line-through;
    }

    .inprogress {
        color: black;
    }

    .box {
        width: auto;
        height: 200px;
        border-radius: 5px;
        background: #22d686;
        color: white;
        font-weight: bold;
        font-size: 40px;
        text-align: center;
    }

    .box * {
        position: relative;
        /*top: 40%;*/
    }

</style>
