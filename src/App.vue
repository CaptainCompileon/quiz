
// TODO: on change of selectedted element propagate change from SelectedArea component to App component

<template>
  <div id="app">
    <Header
    :numCorrect="numCorrect"
    :numTotal="numTotal"
    :getQuestionIndex="getQuestionIndex"
    />

    <b-container class="bv-example-row">
  <b-row>
    <b-col lg="6"> 
         <SelectArea
          :category="category"
          :difficulty="difficulty"
          @updateApiUrlEvent="updateApiUrl($event)"
         />
    </b-col>
   
    <b-col lg="6" > 
         <QuestionBox
          v-if="questions.length"
          :currentQuestion="questions[index]"
          :next="next"
          :increment="increment"
          :questionIndex="index"
          @fetchNewQuestionsEvent="fetchNewQuestions"
         />
    </b-col>
    
  </b-row>
</b-container>
  </div>
</template>

<script>
import Header from './components/Header'
import QuestionBox from './components/QuestionBox'
import SelectArea from './components/SelectArea'

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox,
    SelectArea
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      category: 'any',
      difficulty: 'any'
    }
  },
  methods: {
    getQuestionIndex() {
      return this.index
    },
    next() {
      this.index++
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      }
      this.numTotal++
    },
    getApiUrl() {
      let category = (this.category === 'any') ? '' : '&category=' + this.category
      let difficulty = (this.difficulty === 'any') ? '' : '&difficulty=' + this.difficulty
      
      let url = 'https://opentdb.com/api.php?amount=10'
              + category + difficulty + '&type=multiple'

      return url
    },
    updateApiUrl: function([category, difficulty]) {
      if (category !== null) this.category = category
      if (difficulty !== null) this.difficulty = difficulty

      this.fetchNewQuestions()
    },
    fetchNewQuestions: function() {
          this.$nextTick(function () {

              fetch(this.getApiUrl(), { 
              method: 'get'
              })
                .then((response) => {
                  return (response.json())
                })
                  .then((jsonData) => {
                    this.questions = jsonData.results
                  })
          })
    }
  } ,
  mounted: function() {
    this.fetchNewQuestions()
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
