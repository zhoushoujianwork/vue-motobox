<template>
  <div class="view">
    <!-- <input type="text" v-model="msg"><button @click="send">发言</button>
    <div class="chat-title">聊天记录：</div>
    <div v-for="(item,index) in msgList" :key="index" class="chat-box">{{item}}</div> -->
  </div>
</template>
 
<script>
  export default {
    data() {
      return {
        // ws: '',
        // a:[],
        // A:[],
        // w:[],
      }
    },
    methods: {
      send() {
        this.ws.send(this.msg)
        // this.ws.send(JSON.stringify({msg: this.msg}))
        this.msg = ''
      }
    },
    mounted() {
      this.ws = new WebSocket('ws://192.168.1.23:3000/ws')
      // 连接打开时触发
      this.ws.onopen = () => {  
        console.log("Connection open ...") 
      }
      // 接收到消息时触发  
      this.ws.onmessage = (evt) => { 
        // var _data = eval(evt.data)
        console.log(JSON.parse(evt.data))
      } 
      this.ws.onclose = () => {
        console.log('Connection close !!!')
      }
    },
    // 关闭连接 
    beforeDestroy() {
      this.ws.close()
    }
  }
</script>
 
<style scoped>
.view{
  width: 600px;
  margin: 0 auto;
  background-color:aliceblue;
  height: 500px;
  text-align: center;
  padding-top: 80px;
}
.chat-title{
  text-align:left;
  margin-left:100px;
  margin-top:50px;
}
.chat-box{
  background-color: white;
  width: 400px;
  margin: 0 auto;
}
</style>