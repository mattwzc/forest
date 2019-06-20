<template>
<section class="hero is-success is-fullheight">
  <div class="hero-body">
    <div class="container">
      <div id="welcome">
        <h1 class="title" style="font-size: 3em">
          {{ started ? '正在种植中...' : '开始种树吧' }}
        </h1>
        <h2 class="subtitle">
          {{ started ? 'Planting...' : 'Start planting your tree. ' }}<i class="fas fa-tree"></i>
        </h2>
      </div>
      <!-- add some space -->
      <div id="spacer"></div>
        <div class="columns col">
          <div class="column smalltree">
            <figure class="image is-128x128 is-flex is-middle">
              <img src="../assets/smalltree.png">
            </figure>
          </div>
          <div v-if="!started" class="column" id="clock">
            <circle-slider
                v-model="message"
                :side="150"
                :min="0"
                :max="60"
                :step-size="1"
                :circle-width="12"
                :progress-width="12"
                :knob-radius="10"
                :progressColor="`#5ae26a`"
                :knobColor="`#5ae26a`"
                :circleColor="`#c5c946`"
            ></circle-slider>
          </div>
        </div>
      
      <h1 id="countdown">{{started ? "“即使爬到最高的山上，一次也只能脚踏实地地迈一步”": display()}}</h1>
      <div v-if="started">
        <countdown :time=" time * 60 * 1000 " @end="end" >
        <p class="countdown" slot-scope="props">  {{ props.minutes }} : {{ props.seconds }}</p>
        </countdown>
      </div>
      <b-button @click="started = ! started ; setTimer()" class="is-danger is-large" id="button1" icon-left="play-circle">{{ started ? '停止种植' : '开始种树' }}
</b-button>
    </div>
  </div>
 

</section>





</template>



<script>
import Vue from 'vue'
import Buefy from 'buefy'
import VueCircleSlider from 'vue-circle-slider'
import 'buefy/dist/buefy.css'
import VueCountdown from '@chenfengyuan/vue-countdown';


Vue.component(VueCountdown.name, VueCountdown);

Vue.use(Buefy)
Vue.use(VueCircleSlider)
export default {
  name: 'Tree',
  props: {
    msg: String
  },

  data() {
          return {
              message: 25,
              started : false,
              time: null,
          };
      },

  methods: {
    setTimer() {
      this.time = this.message;

    },

    display() {
      return "倒计时 "+this.message+" 分钟";
    },

    end(){
      console.log("ended")
      this.$dialog.confirm({
                    message: '恭喜，你用 '+ this.message +' 分钟的时间种活了一棵树。',
                    onConfirm: () => this.$toast.open('User confirmed')
                })
    }
  }
}



</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>


#welcome  {

}

.is-middle  {
  text-align: center;
  margin: 0 auto;
}

#countdown {
  margin-top: 160px
}

#button1  {
  margin: 20px;
}

.columns{
   
  
}

.column{
  width: 100%;
  margin-top: 90px
}

.smalltree  {
position:absolute; top:0; left:0;
}

#clock {
position:absolute; top:0; left:0;
}

#spacer  {

height:50px;

}

.hero{
  background-color: green;
}

.countdown{
  font-size: 8em;
}
</style>
