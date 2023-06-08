<template>
   <v-app id="inspire">
    <v-main class="bg-grey-lighten-3">      
      <v-container>
         getOpenAi
        <v-btn
          large
          block
          class="mt-2 green--text" 
          color="white"
          @click="getOpenAi"
        >
          Receive
        </v-btn>           
      </v-container>
    </v-main>
  </v-app>  
</template>

<script>
 
import { ChatGPTAPI } from 'chatgpt'

export default {
  name: 'App',
  methods: {
    async getOpenAi() {
      const api = new ChatGPTAPI({
        apiKey: ''
      })

      const res = await api.sendMessage('Show me how to use cosmJs')
      console.log(res.text)     
      
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
}
</style>
