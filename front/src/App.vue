<template>
  <div id="app">
    <!-- <div class="logo">
      <img alt="Vue logo" src="./assets/logo-hako.png">
      <h1>
        <span class="blue">hako</span>
        <span class="orange">bio</span>
      </h1>
    </div> -->
    
    <Chatbot />
  </div>
</template>

<script>
import Chatbot from './components/Chatbot.vue'

export default {
  name: 'app',
  components: {
    Chatbot
  }
}
</script>

<style>
html, body{
  margin: 0;
}
#app {
  background: url("assets/capture.png") top/cover;
  width: 100vw;
  height: 100vh;
  margin: 0;
  display: flex;
  align-items: flex-end;
}

.blue{
  color: #00B3B3;
}
.orange{
  color: #FF8047;
}
</style>
