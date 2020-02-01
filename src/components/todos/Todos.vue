<template>
    <div class="todos-block">
        <h2>todos</h2>
        <div class="main">
            <button type="button" class=" btn btn-sm dropdown-toggle dropdown-toggle-split" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                <span class="sr-only">Toggle Dropdown</span>
            </button>
            <input class="input-todos" type="text" placeholder="What needs to be done?" @keyup.enter="addTodo">
            <div v-if="todosList.length">
                <Todo
                    v-for="(todo, index) of todosList"
                    :key="index"
                    :index="index"
                    :prop="todo"
                    @ondelete="onDeleteTodo(index)"
                ></Todo>
                <div class="todos-footer">
                    <span class="length-list">{{todosList.length}} item left</span>
                    <button class="button-footer">All</button>
                    <button class="button-footer">Active</button>
                    <button class="button-footer">Done</button>
                    <button class="button-footer clear-all" @click="onClearDone">Clear Done</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Todo from "./Todo.vue"

export default {
    name: "Todos",
    components: {
        Todo
    },
    data() {
        return {
            todosList: [],
        }
    },
    methods: {
        addTodo(event) {
            this.todosList.push({
                text: event.target.value,
                done: false,
                id: null,
            });
            event.target.value = "";
        },
        onDeleteTodo(index) {
            this.todosList.splice(index, 1);
        },
        onClearDone() {
            
        }
    },
}
</script>

<style scoped>

.todos-block {
    display: inline-block;
    background-color: rgb(255, 241, 164);
    width: 300px;
    height: 300px;
}

h2 {
    color: rgba(255, 0, 0, 0.486)
}

.main {
    background-color: #fff;
    margin: 5px auto;
    width: 250px;
}

.input-todos {
    border: none;
}


.open-split:focus {
    outline: none;
}

.todos-footer {
    border: 1px solid rgb(177, 177, 177);
    width: 100%;
    height: 20px;
    padding: 0;
}

.length-list {
    float: left;
    font-size: 10pt;
    margin: 0 10px 0 5px;
    color: rgb(134, 134, 134);
}

.button-footer {
    background-color: #fff;
    height: 15px;
    font-size: 10pt;
    border: none;
    margin: 0;
    padding: 0 5px;
    float: left;
}

.clear-all {
    float: right;
}

</style>