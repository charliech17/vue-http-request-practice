<template>
  <learning-survey @submitClick="dataSubmit"></learning-survey>
  <!-- <learning-survey @survey-submit="storeSurvey"></learning-survey> -->
  <user-experiences :pressLoading="isSubmit" @pressFinish="resetSubmit"></user-experiences>
  <!-- <user-experiences :results="savedSurveyResults"></user-experiences> -->
</template>

<script>
import LearningSurvey from './components/survey/LearningSurvey.vue';
import UserExperiences from './components/survey/UserExperiences.vue';

export default {
  components: {
    LearningSurvey,
    UserExperiences,
  },
  data(){
    return {
      isSubmit:false
    };
  }
  ,methods:{
    dataSubmit(){
      this.isSubmit = true;
      // this.isSubmit = false;
    },resetSubmit(){
      this.isSubmit = false;
    }
  }
  // data() {
  //   return {
  //     savedSurveyResults: [],
  //   };
  // },
  // methods: {
  //   storeSurvey(surveyData) {
  //     const surveyResult = {
  //       name: surveyData.userName,
  //       rating: surveyData.rating,
  //       id: new Date().toISOString(),
  //     };
  //     this.savedSurveyResults.push(surveyResult);
  //     console.log(surveyResult);
  //   },
  // },
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