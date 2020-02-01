<template>
    <div class="todo-prop">
        <input class="check-box" type="checkbox" v-model="checked">
        <span :id="idTodo" class="text-todo">{{prop.text}}</span>
        <button class="button-delete-todo" @click="onDelete">X</button>
    </div>
</template>

<script>
export default {
    name: "Todo",
    props: {
        index: Number,
        prop: Object,
    },
    data() {
        return {
            checked: false,
            id: this.index,
            idTodo: Math.floor(Math.random()*100000),
        }
    },
    methods: {
        onDelete() {
            this.checked = false;
            return this.$emit('ondelete')
        }
    },
    watch: {
        checked: function(bul) {
            this.prop.done = !this.prop.done;
            let text = `#${this.idTodo}`;

            if (bul) {
                this.$el.getElementById(`${this.idTodo}`).classList.add("text-todo-line");
            } else {
                this.$el.getElementIdBy(text).classList.remove("text-todo-line");
            }
        },
        id() {
            this.prop.id = this.index;
        }
    },
    mounted() {
        this.prop.id = this.index
    }
}
</script>

<style scoped>

.todo-prop {
    background-color: #fff;
    border: 1px solid rgb(177, 177, 177);
    border-top: none;
    width: 100%;
    height: 40px;
    display: grid;
    grid-template-columns: 1fr 5fr 1fr;
}

.text-todo {
    margin: auto;
    margin-left: 10px;
}

.text-todo-line {
    text-decoration-line: line-through;
}

.check-box {
    margin: auto;
    border-radius: 20%;
    width: 20px;
    height: 20px;
    background-color: #fff;
}

.button-delete-todo {
    background-color: #fff;
    margin: auto;
    padding: auto;
    width: 30px;
    height: 30px;
    font-size: 12pt;
    border: none;
    color: rgba(255, 0, 0, 0.274);
}


.button-delete-todo:hover {
    color: red;
}


</style>