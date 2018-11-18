<template>
    <div>
        <h3>{{title}}</h3>
        <input type="text" v-model="new_todo"><input type="button" value="ADD" v-on:click="addTodo">
        <ul>
            <TodoItem
                    v-for="todo in user.todo_list"
                    v-bind:todo="todo"
                    v-bind:key="todo.id"></TodoItem>
        </ul>
    </div>
</template>
<script>
    import axios from 'axios';
    import TodoItem from "./TodoItem";

    export default {
        props: [ 'user' ],
        components: { TodoItem },
        data() {
            return {
                title: "Todo List",
                new_todo: undefined
            }
        },
        methods: {
            addTodo: async function () {
                if (this.new_todo) {
                    await axios
                        .post('http://127.0.0.1:3000/api/task/new', {
                            name: this.new_todo,
                            is_done: false
                        });
                    this.refresh();
                    this.new_todo = undefined;
                }
            },
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