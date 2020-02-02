<template>
    <div class="todo-prop" :class="classesProp" @mouseenter="onEnterProp" @mouseleave="onLeaveProp">
        <input class="check-box" type="checkbox" :checked="prop.done" @change="() => $emit('toggle', !prop.done)">
        <span :id="idTodo" class="text-todo" :class="classesText">{{prop.text}}</span>
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
            id: this.index,
            idTodo: Math.floor(Math.random()*100000),
            classesText: [],
            classesProp: [],
            checkEnter: false,
        }
    },
    methods: {
        onDelete() {
            this.checked = false;
            return this.$emit('ondelete')
        },
        onEnterProp(event) {
            event.target.querySelector("button").classList.add("button-delete-todo-vision")
        },
        onLeaveProp(event) {
            if (event.target.querySelector("button").classList.contains("button-delete-todo-vision")) {
                event.target.querySelector("button").classList.remove("button-delete-todo-vision")
            }
        }
    },
    watch: {
        "prop.done": function(bul) {
            if (bul) {
                if ( !this.classesText.includes("text-todo-line") ) {
                    this.classesText.push("text-todo-line");
                }
            } else {
                this.classesText.splice(this.classesText.find((item) => item == "text-todo-line"), 1)
            }
        },
    },
    mounted() {
        if (this.prop.done) {
            if ( !this.classesText.includes("text-todo-line") ) {
                    this.classesText.push("text-todo-line");
                }
        }
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
    color: rgb(255, 255, 255);
}

.button-delete-todo-vision {
    color: red;
}

</style>