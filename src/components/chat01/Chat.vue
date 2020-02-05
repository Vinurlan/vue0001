<template>
    <div class="chat-block">
        <h3>MESSAGER</h3>
        <div class="status-block">
            <div class="buttons-load-data">
                <span class="autoload-block badge badge-dark">              
                    Auto: 
                    <button class="button-toggle-autoload btn" :class="!autoreload ? 'btn-outline-danger' : 'btn-outline-success'" @click="autoreload = !autoreload">
                        {{ autoreload ? "ON" : "OFF" }}                  
                    </button>
                </span> 
                <button class="button-refresh btn btn-dark" @click="getDataChat">Refresh</button>
            </div>
        </div>
        <div class="messages-block">
            <Message class="message-prop"
                v-for="(mes, index) of dataChat"
                :key="index"
                :mesprop="mes"
            ></Message>
        </div>
        <div class="input-block">
            <label class="input-name">
                <span class="badge badge-secondary">Name:</span>
                <input class="form-control" id="inputName" type="text" maxlength="8" @keydown.enter.prevent="focusOnTArea">
            </label>
            <textarea class="input-message form-control" ref="textarea" name="inputMessage" id="inputMessage" maxlength="250"             
                @keydown.ctrl.enter="submitMessage"
            ></textarea>
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
            autoreload: false,
            reloadIntervalFunc: Function,
        }
    },
    methods: {
        submitMessage() {
            const dataMessage = {
                name: document.getElementById("inputName").value,
                message: document.getElementById("inputMessage").value,
            };
            
            // Валидация
            if (!dataMessage.name.trim()) {
                document.getElementById("inputName").classList.add("invalidInput");
                return 0;
            } else {
                document.getElementById("inputName").classList.remove("invalidInput")
            }

            if (!dataMessage.message.trim()) {
                document.getElementById("inputMessage").classList.add("invalidInput")
                return 0;
            } else {
                document.getElementById("inputMessage").classList.remove("invalidInput")
            }

            document.getElementById("inputMessage").value = "";

            fetch("https://basebackpack.firebaseio.com/data-chat.json", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify(dataMessage)
            })
                .then(response => response.json())
                .then(() => setTimeout(this.getDataChat()), 3000)
        },
        getDataChat() {
            fetch("https://basebackpack.firebaseio.com/data-chat.json")
                .then(response => response.json())
                .then(data => Object.values(data).length ? this.dataChat = Object.values(data) : data)
                .catch(data => this.dataChat = Object.values(data))
                .then(() => document.querySelector(".messages-block").scrollTo(0, document.querySelector(".messages-block").scrollHeight));
                 console.log(this.autoreload);
        },
        focusOnTArea() {
            this.$refs.textarea.focus();
        },
        switchAutoReload() {
            new Promise(() => {this.getDataChat()})
                .then(() => {
                    if (this.dataChat.length && this.autoreload) {
                        this.reloadIntervalFunc = setInterval(() => this.getDataChat(), 2000);
                    } else {                      
                        clearInterval(this.reloadIntervalFunc);
                    }       
            })
        },
    },
    watch: {
        autoreload() {
            this.switchAutoReload();
        }
    }
}
</script>

<style scoped>

.chat-block {
    margin: 50px auto;
    background-color: rgb(58, 73, 88);
    border: 4px solid #000;
    border-radius: 5px;
    max-width: 800px;
    height: 400px;
    display: grid;
    grid-template-rows: 1fr 1fr 4fr 2fr;
    transition: max-width 1s;
}


h3 {
    margin: auto;
    color:rgb(255, 255, 255)
}

.status-block {
    background-color: rgb(89, 173, 127);
    width: 100%;
    border-bottom: 1px solid #000;
    border-radius: 0 0 10px 0px;
    padding: 2px 0;
}

.buttons-load-data {
    float: right;
    height: 100%;
    margin: auto 10px;
    padding: 0;
}

.autoload-block {
    font-size: 10pt;
    height: 100%;
    padding: auto;
    margin:  auto 2px;
    float: left;
}

.button-toggle-autoload {
    height: 100%;
    font-size: 10pt;
    padding: 5px 10px;
    margin: auto;
}

.button-refresh {
    height: 100%;
}


.messages-block {
    background-color: rgb(255, 255, 255);
    overflow: scroll;
    overflow-x: hidden; 
    scroll-behavior: smooth;
}

.input-block {
    background-color: rgb(89, 173, 127);
    width: 100%;
    border-top: 1px solid #000;
    border-radius: 0 10px 0 0;
}

.input-name {
    float: left;
    margin: 45px 0 45px 10px;
    transition: visibility 1s;
}

.input-message {
    float: left;
    margin: 10px;
    resize: none;
    width: 55%;
    height: 130px;
}

.submit-message {
    float: left;
    margin: 10px 10px 10px 0;
    font-size: 20pt;
    padding: 43px 11px;
}

.invalidInput {
    border: 1px outset rgba(255, 0, 0, 0.445);
}


@media (max-width: 810px) {
    .chat-block {
        max-width: 400px;
        grid-template-rows: 1fr;
    }

    .input-block {
        background-color: rgb(209, 207, 207);
    }

    
    .input-name {
        display: none;
    }

    .input-message {
        margin: 10px;
        width: 68%;
        height: 130px;
    }

    .submit-message {
        font-size: 20pt;
        padding: 43px 5px;
    }
}

@media (max-width: 410px) {
    .chat-block {
        max-width: none;
    }

    .input-block {
        width: 100%;
        display: flex;
        justify-content: center;
    }


    .input-message {
        float: left;
        margin: 10px;
        width: 68%;
        height: 75px;

    }

    .submit-message {
        font-size: 15pt;
        margin: 10px auto;
        padding: 15px 5px;
    }
}

</style>