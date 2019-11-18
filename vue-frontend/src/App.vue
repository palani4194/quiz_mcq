<template>
  <div id="app">
    <Header
  :corectAnswer = "corectAnswer"
  :total = "total"

    />

    <b-container class="bv-example-row">
      <b-row>
      <b-col offset="3" sm="6">

        <QuestionBox
        v-if="questions.length"
        :singleQuestion="questions[index]"
        :next="next"
        :prev="prev"
        :increment="increment"/>

      </b-col>
      </b-row>
  </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox,
  },
  data(){
    return{
      questions :[],
      index : 0,
      corectAnswer :0,
      total : 0

    }
  },
  methods:{
    next(){
      this.index ++;
    },
    prev(){
      this.index --;
    },
    increment(isCorrect){
      if(isCorrect){
        this.corectAnswer ++ ;
      }
      this.total ++;
    }
  },

  // get questions data from api
  mounted() {
    fetch("https://opentdb.com/api.php?type=multiple&amount=20&category=27",
    {
      method:'get'
    })
    .then((response)=>{
      return response.json()
    })
    .then((jsonData)=>{
      this.questions = jsonData.results
    })

},


}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
