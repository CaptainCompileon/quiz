<template>
    <b-container>
        <b-jumbotron header="Quiz App">
            <template #lead>
                <b-container>
                   <b-row class="text-center">
                        <b-col cols="7">Overall Score:</b-col>
                        <b-col cols="3">
                            <span id="correctAnswers">{{numCorrect}}</span> / {{numTotal}}</b-col>
                   </b-row>
                </b-container>
                <b-container>
                   <b-row class="text-center">
                        <b-col cols="7">This Quiz Set Session:</b-col>
                        <b-col cols="3">
                            <span id="correctAnswers">{{numCorrectSession}}</span> / 10</b-col>
                   </b-row>
                </b-container>
            </template>
        </b-jumbotron>
    </b-container>
</template>


<script>
export default {
    props: [
        'numCorrect',
        'numCorrectSession',
        'numTotal'
    ]
}
</script>

<style scoped>
    #correctAnswers {
        color: rgb(87, 224, 62);
    }

    .lead {
        margin-top: auto;
    }

    .jumbotron {
        padding: 1rem 2rem;
    }
</style>