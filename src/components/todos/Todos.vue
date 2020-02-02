<template>
    <div class="todos-block">
        <h2>todos</h2>
        <div class="main">
            <div class="main-input">
                <div class="btn-group button-split" :class="visionListToggle ? 'dropdown' : 'dropright'">
                    <button type="button" class="btn btn-sm dropdown-toggle dropdown-toggle-split" @click="splitTodos">
                        <span class="sr-only">Toggle Dropright</span>
                    </button>
                </div>
                <input class="input-todos" type="text" placeholder="What needs to be done?" @keyup.enter="addTodo">
            </div>
            <div v-show="visionListToggle">
                <Todo
                    v-for="(todo, index) of visionList"
                    :key="index"
                    :index="index"
                    :prop="todo"
                    @toggle="val => todo.done = val"
                    @ondelete="onDeleteTodo(index)"
                ></Todo>
                <div class="todos-footer">
                    <span class="length-list">{{todosList.length}} item left</span>
                    <button class="button-footer" @click="toAllList">All</button>
                    <button class="button-footer" @click="toActiveList">Active</button>
                    <button class="button-footer" @click="toDoneList">Done</button>
                    <button class="button-footer clear-done" @click="onClearDone">Clear</button>
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
            visionList: [],
            visionListToggle: false,
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
            this.visionListToggle = true;
        },
        onDeleteTodo(index) {
            this.todosList.splice(index, 1);
        },
        toAllList() {
            this.visionList = this.todosList;
        },
        toActiveList() {
            this.visionList = this.todosList.filter((item) => item.done == false);
        },
        toDoneList() {
            this.visionList = this.todosList.filter((item) => item.done == true);
        },
        onClearDone() {
            this.toActiveList();
            this.todosList = this.visionList;        
        },
        splitTodos() {
            if (this.visionList.length) {
                this.visionListToggle = !this.visionListToggle;
            }
        }
    },
    watch: {
        
    },
    mounted() {
        this.toAllList();
    }
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

.main-input {
    display: grid;
    grid-template-columns: 1fr 6fr;
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

.clear-done {
    float: right;
}

</style>