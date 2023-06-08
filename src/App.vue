<template>
   <v-app id="inspire">
    <v-main class="bg-grey-lighten-3">      
      <v-container>
         <HelloWorld msg="Welcome to Your Vue.js App"/>
         <!--<OpenAi />-->
      </v-container>
    </v-main>
  </v-app>  
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
// import OpenAi from './components/openAi.vue'

export default {
  name: 'App',
  components: {
    HelloWorld,
    // OpenAi
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
