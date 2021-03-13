<template>
  <learning-survey @survey-submit="storeSurvey"></learning-survey>
  <user-experiences :results="savedSurveyResults"></user-experiences>
  <!-- REMOVE THE :RESULTS AS BEING HANDLED ON THE SERVER -->
</template>

<script>
import LearningSurvey from './components/survey/LearningSurvey.vue';
import UserExperiences from './components/survey/UserExperiences.vue';

export default {
  components: {
    LearningSurvey,
    UserExperiences,
  },
  //REMOVE THE DATA OBJECT AS BEING HANDLED ON THE SERVER
  data() {
    return {
      savedSurveyResults: [],
    };
  },
  //REMOVE THE METHOD AS BEING HANDLED ON THE SERVER
  methods: {
    storeSurvey(surveyData) {
      const surveyResult = {
        name: surveyData.userName,
        rating: surveyData.rating,
        id: new Date().toISOString(),
      };
      this.savedSurveyResults.push(surveyResult);
      console.log(surveyResult);
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}
</style>