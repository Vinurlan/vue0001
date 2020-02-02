<template>
    <div class="chat-block">
        <h3>MESSAGER</h3>
        <div class="status-block">
            autorefresh
            <button @click="autoreload = !autoreload" disabled>
                {{autoreload ? "ON" : "OFF"}}
            </button>
            <button class="btn btn-dark" @click="getDataChat">Refresh</button>
        </div>
        <div class="messages-block">
            <Message class="message-prop"
                v-for="(mes, index) of dataChat"
                :key="index"
                :mesprop="mes"
            ></Message>
        </div>
        <div class="input-block">
            <label class="input-name"><span class="badge badge-secondary">Name:</span><input class="form-control" id="inputName" type="text"></label>
            <textarea class="input-message form-control" name="inputMessage" id="inputMessage" maxlength="250"></textarea>
            <button class="submit-message btn-sm btn-dark" @click="submitMessage">Enter</button>
        </div>
    </div>
</template>

<script>
import Message from "./Message.vue"

export default {
    name: "Chat",
    components: {
        Message
    },
    data() {
        return {
            dataChat: [],
            autoreload: false
        }
    },
    methods: {
        submitMessage() {
            const dataMessage = {
                name: document.getElementById("inputName").value,
                message: document.getElementById("inputMessage").value,
            };

            document.getElementById("inputName").value = "";
            document.getElementById("inputMessage").value = "";

            fetch("https://basebackpack.firebaseio.com/data-chat.json", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify(dataMessage)
            })
                .then(response => response.json())
                .then(() => setTimeout(this.reloadDataChat()), 3000)
        },
        getDataChat() {
            fetch("https://basebackpack.firebaseio.com/data-chat.json")
                .then(response => response.json())
                .then(data => this.dataChat = Object.values(data))
                .then(() => document.querySelector(".messages-block").scrollTo(0, document.querySelector(".messages-block").scrollHeight))
        },
        reloadDataChat() {
            this.getDataChat();
            setTimeout(this.autoreload ? this.reloadDataChat() : false, 5000)
            
        }
    }
}
</script>

<style scoped>

.chat-block {
    margin: 50px auto;
    background-color: rgb(236, 236, 236);
    border: 1px solid #000;
    width: 800px;
    height: 400px;
    display: grid;
    grid-template-rows: 1fr 1fr 4fr 2fr

}

h3 {
    margin: auto;
}

.status-block {
    background-color: rgb(209, 207, 207);
    width: 100%;
    border-bottom: 1px solid #000;
}

.messages-block {
    background-color: rgb(255, 255, 255);
    overflow: scroll;
    overflow-x: hidden; 
    scroll-behavior: smooth;
}

.input-block {
    background-color: rgb(209, 207, 207);
    width: 100%;

}

.input-name {
    float: left;
    margin: 45px 0 45px 10px;
}

.input-message {
    float: left;
    margin: 10px;
    resize: none;
    width: 440px;
    height: 130px;
}

.submit-message {
    float: left;
    margin: 10px 10px 10px 0;
    font-size: 20pt;
    padding: 43px 14px;
}

</style>