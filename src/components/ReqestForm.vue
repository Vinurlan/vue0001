<template>
    <div class="form-box">
        <button 
            class="select-btn enter" 
            :disabled="formVisible"
            @click="switchPage"
            >Написать</button>
        <button 
            class="select-btn list" 
            :disabled="!formVisible"
            @click="switchPage"
            >Список</button>
        <button class="close-madal close" aria-label="Close" @click.prevent="close"><span aria-hidden="true">&times;</span></button>

        <form class="form-request" v-if="formVisible">
            <input v-model="inputValue.name" id="inputName" type="text" placeholder="Name*">
            <input v-model="inputValue.number" id="inputNumber" type="number" placeholder="Phone*">
            <input v-model="inputValue.comment" id="inputComment" type="text" placeholder="Comment"> 
            <button @click.prevent="submitForm" :disabled="!valid">Отправить</button>     
        </form>
        <div class="list-request" v-if="!formVisible">
            <button @click="onloadRequestList">{{textList}}</button>      
            <div class="list-zone">
                <PropList
                    v-for="(request, index) of requestList"
                    :key="index"
                    :list="request"
                ></PropList>
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
            this.close();
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

        }
    },
    computed: {
        valid() {
            return this.inputValue.name.trim() && this.inputValue.number;
        }
    }
}

</script>

<style scoped>
.form-box {
    position: fixed;
    top: 15%;
    left: 33%;
    background-color: #fff;
    border: 1px solid #000;
    width: 400px;
    height: 500px;
}

.form-request {
    position: absolute;
    top: 25px;
    left: 10px;
    height: 290px;
    width: 250px;
    padding: 90px 55px 50px;
    margin: 10px;
}

input {
    width: 200px;
    margin: 30px 0;
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
    border: none;
    padding-top: 4px;
}

.list {
    left: 75px;
}

.close-madal {
    position: absolute;
    top: 0;
    right: 0;
    margin: 10px;
}

.list-request {
    position: absolute;
    top: 25px;
    left: 10px;
    height: 340px;
    width: 250px;
    padding: 50px 55px;
    margin: 10px;
}

.list-zone {
    overflow: scroll;
    overflow-x: hidden;
    padding-left: 19px;
    width: 90%;
    height: 100%;
}

</style>