<template>
  <div class="training">
   <h1>Math training. Level {{ level + 1}} </h1>
   <hr>
   <div class="progress">
    <div class="progress-bar" :style="progressStyles"></div>
    </div>
   <div class="box">
     <transition name="flip" mode="out-in">
     <app-start-screen
    v-if="state == 'start'"
    @onStart="onStart"
    >
    
   </app-start-screen> 
   <app-question v-else-if="state == 'question'"
                 @success="onQuestSuccess"
                 @error="onQuestError"
                 :settings="levels[level]"
   ></app-question>
   <app-message v-else-if="state == 'message'"
                :type="message.type"
                :text="message.text"
                @onNext="onNext"
   ></app-message>
   <app-result-screen v-else-if="state == 'result'"
                      :stats="stats"
                      @repeat="onStart"
                      @nextLevel="onNextLevel"
   ></app-result-screen>
   <div v-else>Unknown state</div>
    </transition>
  </div>
 </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      state: 'start',
      stats: {
        success: 0,
        error: 0
      },
      message: {
        type: '',
        text: ''
      },
      questionMax: 3,
      level: 0,
      levels: [
        {
          from: 10,
          to: 40,
          range: 5,
          variants: 2
        },
        {
          from: 100,
          to: 200,
          range: 20,
          variants: 4
        },
         {
          from: 500,
          to: 900,
          range: 40,
          variants: 6
        }
      ]
    }
  },
  computed: {
    questDone(){
      return this.stats.success + this.stats.error;
    },
    progressStyles(){
      return {
        width: (this.questDone / this.questionMax * 100) + '%'
      }
    }
  },
  methods:{
    onStart(){
      this.state = 'question';
      this.stats.success = 0;
      this.stats.error = 0;
    },
     onQuestSuccess(){
      this.state = 'message';
      this.message.text = 'Good job!';
      this.message.type = 'success';
      this.stats.success++;

     },
     onQuestError(msg){
      this.state = 'message';
      this.message.text = msg;
      this.message.type = 'warning';
       this.stats.error++;
     },
     onNext(){
      if(this.questDone < this.questionMax){
        this.state = 'question';
      }else{
        this.state = 'result';
      }
   },
     onNextLevel(){
      if(this.level  == 2){
      this.level = 0;
      this.state = 'start';
      // console.log('Y' + this.level);
    }else{
        console.log('No' + this.level);
        this.level++;
        this.onStart();
        }
     }
  }
}
</script>

<style scoped>
  .training{
    max-width: 700px;
    margin: 20px auto;
  }
  .progress-bar{
    transition: width 0.5s;
  }
  .box{
    margin: 10px 0;
  }
  .flip-enter{

  }
  .flip-enter-active{
    animation: flipInx 0.3s linear;
  }
  .flip-leave{

  }
  .flip-leave-active{
    animation: flipOutX 0.3s linear;
  }

  @keyframes flipInx{
    from{transform: rotateX(90deg);}
    to{transform: rotateX(0deg);}
  }
  @keyframes flipOutX{
    from{transform: rotateX(0deg);}
    to{transform: rotateX(90deg);}
  }
</style>
