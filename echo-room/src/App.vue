<template>
  <div id="app">
     <div class="wrap">
      <div>
      <h1>WebSockets Demo</h1>
       <p>first connect</p> 

      <div class="function">
        <button @click="connection">Connect</button>
        <button @click="disconnect">Disconnect</button>
      </div>

    

        <form method="post" @submit.prevent="sendMsg">
           <input type="text" v-model="newMsg">
           <button id='send'>send</button>
        </form>

        <p id='msg' v-for="msg in messages">{{ msg }}</p>
        <p v-for="info in informations"> {{ info }}</p>
   

      </div>
    </div>
  </div>
</template>


<script>

export default {
  name: 'app',
  components: {
  },

  data() {
    return {
      informations:[],
      messages:[],
      newMsg:'',
      socket:''
      
    }
  },

  methods: {
    sendMsg() {
      this.socket.send(this.newMsg);
      this.messages.push(this.newMsg)
    },
    
  connection() {
          this.socket = new WebSocket('wss://echo.websocket.org/');
          this.socket.addEventListener('open', () => {
              this.informations.push('connected');
          });
          this.socket.addEventListener('message', (event) => {
              this.messages.push(event.data);
          });
          this.socket.addEventListener('close', () => {
              this.informations.push("disconnected");
          });
    },

    disconnect() {
        this.socket.close();
    },
  }
}
</script>



<style>
#app {
display: flex;
justify-content: center;
padding-top: 5%;
}

.wrap {
  background: lightsalmon;
  height: 80vh;
  width: 80vw;
  display: flex;
  justify-content: center;

}

h1 {
 font-family: 'Quattrocento Sans', sans-serif;
  font-size: 6vh;
  display: inline-block;
}

input {
  background: transparent;
  height: 50px;
  width: 400px;
  margin-top: 70px;
  border: 2px solid #3939ac;
  border-radius: 15px;
  display: block;
}

p {
font-family: 'Quattrocento Sans', sans-serif;
}
button {
  background: transparent;
  color: black;
  font-size: 20px;
  border:2px solid #3939ac;
  margin-right: 10px;

}

#send {
  background: transparent;
  width: 402px;
  border:2px solid #3939ac;
  margin-top: 5px;
  border-radius: 15px;
}


</style>

