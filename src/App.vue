<template>
  <div id="app">
    <div class="indicators">
      <!-- 姿态 -->
      <Attitude class="indicator-bg" :size="size" :roll="roll" :pitch="pitch" />
      <!-- 气压仪器 -->
      <!-- <Variometer class="indicator-bg" :size="size" :vario="vario" /> -->
      <!-- <Altimeter class="indicator-bg" :size="200" :altitude="altitude" :pressure="pressure"/> -->
    </div>
    <div  class="indicators">
      <Heading class="indicator-bg" :size="size" :heading="heading"/>
      <!-- <Airspeed class="indicator-bg" :size="size" :airspeed="airspeed" /> -->
    </div>
  </div>
</template>

<script>
// import {Airspeed, Attitude, Heading, Variometer} from  'vue-flight-indicators'
import {Attitude, Heading} from  'vue-flight-indicators'

export default {
  name: 'app',
  components: {
    Attitude,
    Heading,
    // Variometer,
    // Airspeed,
    // Altimeter
  },
  data: function () {
    return {
      size:200,
      // acc_x:"",
      // acc_y:"",
      // acc_z:"",
      // w_x:"",
      // w_y:"",
      // w_z:"",
      // angle_x:"",
      // angle_y:"",
      // angle_z:"",
      heading:0,
      // vario:0,
      pitch:0,
      roll:0,
      // airspeed:0,
    }
  },
  mounted() {
    this.ws = new WebSocket('ws://192.168.1.23:3000/ws')
    // 连接打开时触发
    this.ws.onopen = () => {  
      console.log("Connection open ...") 
    };
    // setInterval(()=>{
    //   this.roll = this.counter
    //   // this.counter++
    // },25);
    // 接收到消息时触发  
    this.ws.onmessage = (evt) => { 
      // var _data = eval(evt.data)
      var data = JSON.parse(evt.data)
      // acc_x=data["Acc"]["X"]
      // acc_y=data["Acc"]["Y"]
      // acc_z=data["Acc"]["Z"]
      // w_x=data["W"]["X"]
      // w_y=data["W"]["Y"]
      // w_z=data["W"]["Z"]
      // angle_x=data["Angle"]["X"]
      // angle_y=data["Angle"]["Y"]
      // angle_z=data["Angle"]["Z"]
      
      // this.vario = 2*Math.sin(data["Acc"]["X"]*100);
      this.heading = 36*Math.sin(parseInt(data["Acc"]["X"])/30);

      // pitch  上下      加速度X
      // roll   左右横滚  角度
      // this.pitch = 50*Math.sin((parseInt(data["Acc"]["X"])/20)/2);
      this.roll = Math.sin(parseInt(data["Acc"]["X"]*1000)/60)*90  //放到后端实现数据平滑过度，2个数据间填充数据
      console.log(data["Acc"]["X"],this.roll)
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
#app {
  text-align: center;
  margin-top: 60px;
}
.indicator-bg {
  background-color: grey;
}
</style>