<template>
    <div class="modal-bg bg-opacity">
        <div class="form-box">
            <button 
                class="select-btn enter btn btn-dark" 
                :disabled="formVisible"
                @click="switchPage"
                >Написать</button>
            <button 
                class="select-btn list  btn btn-dark" 
                :disabled="!formVisible"
                @click="switchPage"
                >Список</button>
            <button class="close-madal close" aria-label="Close" @click.prevent.once="closeModal"><span aria-hidden="true">&times;</span></button>

            <form class="form-request" v-if="formVisible">
                <input v-model="inputValue.name" id="inputName" type="text" placeholder="Name*">
                <input v-model="inputValue.number" id="inputNumber" type="number" placeholder="Phone*">
                <input v-model="inputValue.comment" id="inputComment" type="text" placeholder="Comment"> 
                <button @click.prevent="submitForm" :disabled="!valid">Отправить</button>     
            </form>
            <div class="list-request" v-if="!formVisible">
                <button class="reset-btn btn btn-dark" @click="onloadRequestList">{{textList}}</button>      
                <div class="list-zone">
                    <PropList
                        v-for="(request, index) of requestList"
                        :key="index"
                        :list="request"
                    ></PropList>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import PropList from "./PropRequest.vue";

export default {
    name: "ReqestForm",
    props: {
        close: Function
    },
    components: {
        PropList
    },
    data() {
        return {
            formVisible: true,
            requestList: [],
            inputValue: {name: "", number: "", comment: ""},
            textList: "Обновить",
        }
    },
    methods: {
        switchPage() {
            this.formVisible = !this.formVisible;
        },
        submitForm() {
            const requestData = {
                name: this.inputValue.name,
                number: this.inputValue.number,
                comment: this.inputValue.comment
            };

            fetch("https://basebackpack.firebaseio.com/client-request.json", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify(requestData)
            })
                .then(response => response.json);
            this.inputValue = {name: "", number: "", comment: ""};
            this.$emit('closemodal');
        },
        onloadRequestList() {
            this.requestList = [];
            this.textList = "Загрузка";
            fetch("https://basebackpack.firebaseio.com/client-request.json", {
                method: "GET",
                headers: {
                    "Content-Type": "application/json"
                }
            })
                .then(response => response.json())
                .then(response => response != null || response != undefined ? this.requestList = Object.values(response) : response)
                .then(response => response != null || response != undefined ? this.textList = "Обновить" : this.textList = "База пуста")
                .then(() => {
                    if (this.requestList != 0) {
                        this.$nextTick(() => document.querySelector(".list-zone").style.overflow = "");
                    }
                })
        },
        closeModal() {
            new Promise((resolve) => {
                document.querySelector(".modal-bg").classList.add("bg-opacity")
                setTimeout(() => resolve(), 200)
            }).then(() => this.$emit('closemodal'))
        },
    },
    computed: {
        valid() {
            return this.inputValue.name.trim() && this.inputValue.number;
        }
    },
    watch: {
        "formVisible": function() {
            if (this.formVisible == false && this.requestList.length == 0) {
                this.$nextTick(() => document.querySelector(".list-zone").style.overflow = "hidden");
            } else {
                if (this.formVisible == false) {
                    this.$nextTick(() => document.querySelector(".list-zone").style.overflow = "");
                }
            }
        }
    },
    mounted() {
        this.$nextTick(() => document.querySelector(".modal-bg").classList.remove("bg-opacity"));
    },
}

</script>

<style scoped>
.modal-bg {
    z-index: 2;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.205);

    display: flex;
    justify-content: center;
    align-items: center;
    opacity: 100%;

    transition: opacity 0.2s;
}

.bg-opacity {
    opacity: 0;
}

.form-box {
    position: absolute;
    right: 0;
    left: 0;
    margin: 0 auto;
    background-color: #fff;
    border: 1px solid #000;
    width: 400px;
    height: 500px;

    display: flex;
    justify-content: center;
    align-items: center;
}

.form-request {
    position: absolute;
    display: grid;
    justify-content: center;
    right: 0;
    left: 0;
    height: 290px;
    width: 250px;
    padding: 0;
    margin: 0 auto;
}

input {
    width: 200px;
    margin: 20px 0;
}

.form-request button {
    background-color: #000;
    border: none;
    margin: 20px 15px 0;
    color: #fff;
    transition: background-color 300ms, color 300ms;
}

.form-request button:disabled,
.form-request button:hover {
    background-color: #fff;
    color: #000;
}

.select-btn {
    position: absolute;
    top: 0;
    left: 0;
    margin: 10px;
}

.select-btn:disabled {
    background-color: #fff;
    color: #000;
    border: none;
    padding-top: 8px;
    cursor: default;
} 

.list {
    left: 100px;
}

.close-madal {
    position: absolute;
    top: 0;
    right: 0;
    margin: 10px;
    width: 30px;
    height: 30px;
}

.list-request {
    position: absolute;
    right: 0;
    left: 0;
    height: 340px;
    width: 250px;
    padding: 0;
    margin: 0 auto;
}

.list-zone {
    overflow: scroll;
    overflow-x: hidden;
    padding-left: 19px;
    width: 90%;
    height: 100%;
}

</style>