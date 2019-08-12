<template>
  <div id="app">
    <Header
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    ></Header>

    <QuestionBox
      :currentQuestion = "questions[index]"
      :next="next"
      :increment="increment"
    ></QuestionBox>

  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox
  },
  data: function()
  {
    return{
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods:{
    next: function(){
      if(this.index<10)
      {
        this.index++
      }

    },

    increment(isCorrect){
      if(isCorrect)
      {
        this.numCorrect++
      }
      this.numTotal++;
    }
  },

  mounted: function () {
    fetch('https://opentdb.com/api.php?amount=10&category=23&type=multiple', {
      method: 'get'
    })
            .then((response) => {
              return response.json();
            })

            .then((jsonData) => {
              this.questions = jsonData.results;
            })
  }
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
