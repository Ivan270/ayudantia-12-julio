<template>
  <div id="App">
    <div class="container">
      <div class="row justify-content-center mb-3 gap-4">
        <!-- Usuario 1 -->
        <div class="card col-2">
          <img :src="firstUser?.picture.large" alt="">
          <div class="card-body">
            <h5 class="card-title">{{ firstUser?.name?.first }} {{ firstUser?.name?.last }}</h5>
            <form id="primera"
              @submit.prevent="sendMessageHandler(firstUser?.id?.value, firstUserMessage, firstUser?.name?.first, firstUser?.name?.last)">
              <input class="form-control" type="color" name="color" id="color" v-model="firstUserColor">
              <input class="form-control" type="text" name="message" id="message" v-model="firstUserMessage">
              <div class="d-grid">
                <button type="submit" class="btn btn-outline-light btn-sm">Send</button>
              </div>
            </form>
          </div>
        </div>
        <!-- Cuadro del chat -->
        <div id="chatBox" class="border border-secondary p-2 col-5 rounded">
          <ChatBox :messages="messages" :firstUser="firstUser" :secondUser="secondUser" />
        </div>

        <!-- Usuario 2 -->
        <div class="card col-2">
          <img :src="secondUser?.picture.large" alt="">
          <div class="card-body">
            <h5 class="card-title">{{ secondUser?.name?.first }} {{ secondUser?.name?.last }}</h5>
            <form id="primera"
              @submit.prevent="sendMessageHandler(secondUser?.id?.value, secondUserMessage, secondUser?.name?.first, secondUser?.name?.last)">
              <input class="form-control" type="color" name="color" id="color" v-model="secondUserColor">
              <input class="form-control" type="text" name="message" id="message" v-model="secondUserMessage">
              <div class="d-grid">
                <button type="submit" class="btn btn-outline-light btn-sm">Send</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import ChatBox from '@/components/ChatBox.vue'

export default {
  name: 'App',
  data() {
    return {
      users: [],
      firstUserColor: '#075E54',
      firstUserMessage: '',
      secondUserColor: '#128C7E',
      secondUserMessage: '',
      messages: []
    }
  },
  methods: {
    async buscarUsers() {
      try {
        let response = await axios.get('https://randomuser.me/api/?results=2');
        this.users = response.data.results
      } catch (error) {
        console.log(error)
      }
    },
    sendMessageHandler(id, message, firstName, lastName) {
      const newMessage = {
        userId: id,
        message: message,
        color: id == this.firstUser.id.value ? this.firstUserColor : this.secondUserColor,
        name: firstName,
        last: lastName
      }
      this.messages.push(newMessage)
      // Limpiar form
      if (id == this.firstUser.id.value) {
        this.firstUserMessage = ''
      } else if (id == this.secondUser.id.value) {
        this.secondUserMessage = ''
      }
    },
  },
  computed: {
    firstUser() {
      return this.users[0]
    },
    secondUser() {
      return this.users[1]
    }
  },
  components: {
    ChatBox,
  },
  // Lifecycle hooks
  created() {
    this.buscarUsers();
  }
}
</script>

<style>
#App{
  min-height: 100vh;
  display: flex;
  place-items: center
}

#chatBox{
  min-height: 50vh;
  overflow-y: auto;
  max-height: 50vh;
}
.card{
  height: 100% !important;
}


</style>
