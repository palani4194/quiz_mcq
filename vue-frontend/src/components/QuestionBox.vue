<template>
  <div>
  <b-jumbotron>

    <b>
    {{singleQuestion.question}}
  </b>
    <hr class="my-4">

    <b-list-group class="py-2"  v-for="(answer,index) in suffledAnswers" :key="index">
      <b-list-group-item @click="selectedOption(index)"
      :class="answeredClass(index)">  {{answer}}</b-list-group-item>
    </b-list-group>


    <b-button  variant="success" @click="prev">Prev</b-button>
    <b-button variant="primary"  @click="submit" :disabled="selectedIndex === null || answered === true" >Submit</b-button>
    <b-button variant="success" @click="next">Next</b-button>


  </b-jumbotron>
</div>
</template>

<script>
import _ from 'lodash';

export default
{
  props:
  {
    singleQuestion :Object,
    next :Function,
    prev : Function,
    increment: Function,


  },

  data(){
    return{
      selectedIndex:null,
      correctIndex : null,
      suffledAnswers:[],
      correctAnswers : 0,
      totalcount : 0,
      answered : false,

    }
  },

  methods:{
    // get selected option
    selectedOption(index){
      this.selectedIndex = index
    },

    // suffle answers by using lodash js library
    suffledAnswer(){
      let answers = [ ... this.singleQuestion.incorrect_answers,this.singleQuestion.correct_answer]
      this.suffledAnswers = _.shuffle(answers)
      this.correctIndex  = this.suffledAnswers.indexOf(this.singleQuestion.correct_answer)
    },

    // submit the answers
    submit(){
      let isCorrect = false
      if(this.selectedIndex === this.correctIndex){
        isCorrect = true
      }
      this.answered = true
      this.increment(isCorrect)
    },

    // change the styles based on answers

    answeredClass(index){
      // debugger;

      let answeredClass = ''

      if(!this.answered && this.selectedIndex === index){
        answeredClass  = 'selected-ans'
      }
      else if(this.answered && index === this.correctIndex){
        answeredClass  = 'correct-ans'
      }
      else if(this.answered && this.selectedIndex === index && this.correctIndex !== index){
        answeredClass  = 'incorrect-ans'
      }
      return answeredClass
    }

  },

// compulte the answer
  computed:{
    answers(){
    let answers = [ this.singleQuestion.correct_answer]
    answers.push(... this.singleQuestion.incorrect_answers)
    return answers
  }

  },

  // run watch function every after click the next button
  watch:{
    singleQuestion:{
      immediate:true,
      handler(){
        this.selectedIndex = null
        this.answered = false
        this.suffledAnswer()
      }
    }
  },


}

</script>



<style  scoped lang="postcss">

import '@/assets/css/tailwind.css'


.list-group-item:hover{
  background-color : #e9ecef;
  cursor:pointer;
}
.btn
{
  margin:5px;
}
.selected-ans{
  background-color : lightblue;
}
.correct-ans{
  background-color : lightgreen;
}
.incorrect-ans{
  background-color : red;
}

</style>
