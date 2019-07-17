<template>
    <div class = "container">
        <header class="header"> 
            <h1>
                (Test Client #1): This will broadcast text messages to all chat members
            </h1>
        </header>
        <div class="output">
            <ul height = "100%">
                <li v-for="log in logs" :key="log.id">
                    {{ log }}
                </li>
            </ul>
        </div>
        <form class="input" v-on:submit.prevent="sendmessage" action="#">
            <input type="text" name="message" class="entry-bar" v-model="message">
            <input type="submit" name="submit" value="Submit">
        </form>
    </div>
</template>

<script>
export default {
  name: 'ChatRoom',
  data() {
      return {
          message: "",
          logs: []
      }
  },
  created(){
        console.log("connecting");
        const url = "ws://162.245.217.17:5000";
        this.ws = new WebSocket(url);
        this.ws.onopen = function(event) { console.log("websocket connected successfully!"); };
        this.ws.onmessage = ({data}) => {
            console.log("recieved:" + data);
            this.logs.push(data);
        }
  },
  methods: {
      sendmessage() {
          this.ws.send(this.message);
          this.message = "";
      }
  }
}
</script>

<style scoped>
.container {
    display: grid;
    grid-template-columns: 600px;
    grid-template-rows: 40px 700px 75px;
    grid-template-areas:
        "header"
        "textoutput"
        "textinput";
    grid-row-gap: 20px;

    width: 50%;
    margin: 0 auto;
}

.header {
    grid-area: header;
    place-self: center;
}

.output {
    grid-area: textoutput;
    background-color:#ffb6ad;
    text-align: left;
}

.input {
    grid-area: textinput;
    align-self: stretch;
}

.entry-bar {
    width: 400px;
}

ul {
    list-style-type: none;
}
</style>
