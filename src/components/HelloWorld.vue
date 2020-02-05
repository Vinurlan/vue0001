<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button class="form-request" @click="openMadal">Request</button>
    <RequestForm v-if="visibleMadal" :close="closeMadal"></RequestForm> 
    <Chat></Chat>
    <MenuBar></MenuBar>
    <Todos></Todos>
  </div>
</template>


<script>
import RequestForm from "./ReqestForm.vue"
import Chat from "./chat01/Chat.vue"
import MenuBar from "./MenuBar.vue"
import Todos from "./todos/Todos.vue"

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  components: {
    RequestForm, Chat, MenuBar,
    Todos,
  },
  data () {
    return {
      visibleMadal: false,
    }
  },
  methods: {
    openMadal() {
      this.visibleMadal = true;
    },
    closeMadal() {
      this.visibleMadal = false;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

</style>
