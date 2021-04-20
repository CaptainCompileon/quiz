<template>
    <div class="question-box-container">
        <b-jumbotron>

            <template #lead>
                <div v-html="currentQuestion.question">
                </div>
            </template>

            <hr class="my-4">
            <b-list-group>
                <b-list-group-item
                    v-for="(answer, index) in answers" :key="index"
                    @click.prevent="selectAnswer(index)"
                    :class="answerClass(index)"
                >
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>

            <b-button variant="primary" @click="submitAnswer"
             :disabled="submitButton"
             
             >Submit</b-button>
            <b-button @click="next" variant="success" v-show="questionIndex < 9">Next</b-button>
            <b-button @click="startAgain()" variant="warning" v-show="questionIndex === 9">Start Again</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
    import _ from 'lodash'

    export default {
        props: {
            currentQuestion: Object,
            questionIndex: Number,
            next: Function,
            increment: Function,
        },
        data: function() {
            return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            answered: false,
            submitButton: false
            }
        },
        computed: {
            answers() {
                let answers = [...this.currentQuestion.incorrect_answers]
                answers.push(this.currentQuestion.correct_answer)
                return answers;
            }
        },
        watch: {
            currentQuestion: {
                immediate: true,
                handler() {
                this.selectedIndex = null
                this.answered = false
                this.canBeSubmitted()
                this.shuffleAnswers()
                }
            }
        },
        methods: {
            selectAnswer(index) {
                if (this.submitButton || !this.answered) {
                this.selectedIndex = index
                this.canBeSubmitted()
                }
            },
            shuffleAnswers() {
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
                this.shuffledAnswers = _.shuffle(answers)
                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            },
            submitAnswer() {
                let isCorrect = false

                if (this.selectedIndex === this.correctIndex) {
                    isCorrect = true
                }
                this.answered = true
                this.increment(isCorrect)
            },
            answerClass(index) {
                let answerClass = ''
                
                if ((!this.answered && this.selectedIndex === index)) {
                    answerClass = 'selected'
                }  else if (this.answered && this.correctIndex === index){
                    answerClass = 'correct'
                } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
                    answerClass = 'incorrect'
                } 

                return answerClass
            },
            canBeSubmitted() {
                if (this.selectedIndex === null || this.answered) {
                    this.submitButton = true
                } else this.submitButton = false
            },
            startAgain() {
                this.$emit('fetchNewQuestionsEvent')
            }
        }
    }
</script>

<style scoped>
    .list-group {
        margin-bottom: 15px;
    }
   
    .list-group-item:hover {
        background: rgb(184, 178, 178);
    }

    .btn {
        margin: 0 5px;
    }

    .selected {
        background-color: lightblue;
    }

    .correct {
        background-color: lightgreen;
    }

    .incorrect {
        background-color: red;
    }
</style>