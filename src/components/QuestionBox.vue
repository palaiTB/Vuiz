<template>
    <div class="question-box container">
            <div>
                <b-jumbotron>
                    <template slot="lead">
                       {{currentQuestion.question}}
                    </template>

                    <hr class="my-4">

                    <b-list-group>
                        <b-list-group-item v-for = "(answer,index) in shuffledAnswers" :key="index" @click="selectAnswer(index)" :class="[!answered && selectedIndex === index ? 'selected' : answered && correctIndex === index? 'correct' : answered && selectedIndex===index && correctIndex !== index ? 'incorrect' : '']">
                            {{answer}}
                        </b-list-group-item>
                    </b-list-group>

                    <br>

                    <b-button style="margin-right: 0.5rem" variant="primary" @click= "submitAnswer" :disabled="selectedIndex === null || answered">
                        Submit
                    </b-button>
                    <b-button @click ="next" variant="success" href="#"> Next</b-button>
                </b-jumbotron>
            </div>
        </b-jumbotron>
    </div>
</template>

<script>
    import _ from 'lodash'
    export default{
        props: {
            currentQuestion: Object,
            next: Function,
            increment: Function
        },
        data(){
          return{
              selectedIndex: null,
              shuffledAnswers: [],
              correctIndex: null,
              answered: false
          }
        },
        computed:{

            answers(){
                let answers = [...this.currentQuestion.incorrect_answers]
                answers.push(this.currentQuestion.correct_answer)
                return answers
            }
        },
        watch: {
          currentQuestion(){
              this.selectedIndex = null
              this.answered = false
              this.shuffleAnswers()
          }
        },

        methods: {
          selectAnswer(index){
              this.selectedIndex = index
          },
            shuffleAnswers()
            {
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
                this.shuffledAnswers = _.shuffle(answers)
                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            },

            submitAnswer()
            {   this.answered = true
                let isCorrect = false
                if(this.selectedIndex === this.correctIndex)
                {
                    isCorrect = true
                }
                this.increment(isCorrect)
            }
        },
    }

</script>


<style scoped>
    .selected{
        background-color: #99fff8;
    }

    .correct{
        background-color: lightgreen;
    }
    .incorrect{
        background-color:  orangered;
    }
</style>

