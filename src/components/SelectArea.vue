<template>
    <div class="select-arrea-container">
        <b-jumbotron>
            <div>
              <b-container>
                   <b-row class="text-center">
                        <b-col cols="6">
                            <h4>Category</h4>
                            <b-form-select 
                            v-model="selected1" 
                            :options="optionsCategory"
                            >
                            </b-form-select>
                            <div class="mt-3">Selected: <strong v-html="selectedCategoryText"></strong></div>
                        </b-col>
                        <b-col cols="6">
                            <h4>Difficulty</h4>
                            <b-form-select v-model="selected2" :options="optionsDifficulty"></b-form-select>
                            <div class="mt-3">Selected: <strong v-html="selectedDifficultyText"></strong></div>
                        </b-col>
                   </b-row>
                </b-container> 
            </div>
          
            <b-container id="submitButton">
                <b-button variant="primary" @click="updateApiUrl()"             
             >Submit</b-button>
            </b-container>
            
        </b-jumbotron>
    </div>
</template>

<script>
  export default {
    props: {
        category: String,
        difficulty: String,
  },
    data() {
      return {
        selected1: this.category,
        selected2: this.difficulty,
        selectedCategoryText: 'Any Category',
        selectedDifficultyText: 'Any Difficulty',
        optionsCategory: [
            { value: 'any', text: 'Any Category' },
            { value: '9', text: 'General Knowledge' },
            { value: '10', text: 'Entertainment: Books' },
            { value: '11', text: 'Entertainment: Film' },
            { value: '12', text: 'Entertainment: Music' },
            { value: '13', text: 'Entertainment: Musicals &amp; Theatres' },
            { value: '14', text: 'Entertainment: Television' },
            { value: '15', text: 'Entertainment: Video Games' },
            { value: '16', text: 'Entertainment: Board Games' },
            { value: '17', text: 'Science &amp; Nature' },
            { value: '18', text: 'Science: Computers' },
            { value: '19', text: 'Science: Mathematics' },
            { value: '20', text: 'Mythology' },
            { value: '21', text: 'Sports' },
            { value: '22', text: 'Geography' },
            { value: '23', text: 'History' },
            { value: '24', text: 'Politics' },
            { value: '25', text: 'Art' },
            { value: '26', text: 'Celebrities' },
            { value: '27', text: 'Animals' },
            { value: '28', text: 'Vehicles' },
            { value: '29', text: 'Entertainment: Comics' },
            { value: '30', text: 'Science: Gadgets' },
            { value: '31', text: 'Entertainment: Japanese Anime &amp; Manga' },
            { value: '32', text: 'Entertainment: Cartoon &amp; Animations' }
        ],
        optionsDifficulty: [
          { value: 'any', text: 'Any Difficulty' },
          { value: 'easy', text: 'Easy' },
          { value: 'medium', text: 'Medium' },
          { value: 'hard', text: 'Hard'},
        ]
      }
     },
    methods: {
        updateApiUrl: function(){
         this.$emit('updateApiUrlEvent', [this.selected1, this.selected2])
      },
        showSelectedText: function() {
          let categoryText = this.optionsCategory.find(el => el.value === this.category).text
          this.selectedCategoryText = categoryText

          let difficultyText = this.optionsDifficulty.find(el => el.value === this.difficulty).text
          this.selectedDifficultyText = difficultyText

        }
    },
    watch: {
      category: {
        immediate: true,
        handler() {
          this.showSelectedText()
        }
      },
      difficulty: {
        immediate: true,
        handler() {
          this.showSelectedText()
        }
      }
    },
    mounted: function() {
        this.showSelectedText()
    }
  }
</script>

<style scoped>
    .jumbotron {
        padding: 1rem 2rem;
      }

    #submitButton {
        padding: 1rem 2rem;
    }
</style>