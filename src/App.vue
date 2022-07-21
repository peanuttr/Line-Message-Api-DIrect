<template>
  <div id="app">
    <h1>Broadcast LINE Messaging API</h1>

    <div class="container">
      <div
        v-for="(message, i) in messages"
        :key="i"
        class="message-input">
        <input
          v-model="messages[i]"
          type="text"
          placeholder="message">
        <input
          v-model="userId[i]"
          type="text"
          placeholder="userId">
        <div
          class="remove-button"
          @click="removeMessage(i)">
          Remove
        </div>
      </div>
      <div
        class="add-button"
        @click="addMessage()">
        Add Message
      </div>
      <div
        class="send-button"
        @click="createBroadcast()">
        Send Messages
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data () {
    return {
      messages: [''],
      userId: ['']
    }
  },
  methods: {
    async createBroadcast () {
      try {
        if (this.messages.length > 5) {
          return alert('Error! Message must not more than 5 items.')
        }

        const { data } = await axios.post(`${process.env.VUE_APP_BASE_URL}/push`, {
          to: this.userId,
          messages: this.messages,
        })

        alert(data.message)

        // this.messages = ['']
        // this.userId = ['']
      } catch (error) {
        console.error('createBroadcast', error)
        alert(`Error : ${error.message}`)
      }
    },
    addMessage () {
      this.messages.push('')
    },
    removeMessage (index) {
      if (this.messages.length === 1) {
        this.messages.splice(index, 1, '')
      } else {
        this.messages.splice(index, 1)
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.container {
  width: 30vw;
}
.message-input {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.4rem;
}
.message-input input {
  width: 70%;
  font-size: 1.5rem;
}
.remove-button {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 25%;
  height: 2rem;
  background-color: #c0392b;
  color: white;
  border-radius: 5px;
  cursor: pointer;
  transition-duration: .3s;
}
.remove-button:hover {
  background-color: #e74c3c;
}
.add-button {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 2rem;
  background-color: #27ae60;
  color: white;
  border-radius: 5px;
  cursor: pointer;
  transition-duration: .3s;
  margin-bottom: 0.4rem;
}
.add-button:hover {
  background-color: #2ecc71;
}
.send-button {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 2rem;
  background-color: #2980b9;
  color: white;
  border-radius: 5px;
  cursor: pointer;
  transition-duration: .3s;
}
.send-button:hover {
  background-color: #3498db;
}
</style>
