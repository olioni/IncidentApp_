<template>
  <emailPopup v-if="email" :responsesFromApp="responsesFromApp"/>
  <inputPopup v-if="input" @showPopup="showPopup"/>
  <surveyPopup v-if="survey" @responses="responsesRecieved" @showInput="inputPopup"/>
  <startPopup v-if="start" @beginSurvey="beginSurvey"/>
</template>

<script>
import surveyPopup from './components/surveyPopup.vue'
import emailPopup from './components/emailPopup.vue'
import startPopup from './components/startPopup.vue'
import inputPopup from './components/inputPopup.vue'

export default {
  name: 'App',
  props: [],
  data() {
    return {
      email: false,
      input: false,
      survey: false,
      start: true,

      responsesFromApp: null
    }
  },
  components: {
    surveyPopup,
    emailPopup,
    startPopup,
    inputPopup
  },
  methods: {
    showPopup() {
      this.email = true
      this.input = false
    },
    beginSurvey() {
      console.log('begin')
      this.start = false
      this.survey = true
    },
    responsesRecieved(responses) {
      console.log('emit worked')
      console.log(responses)
      this.responsesFromApp = responses
    },
    showInput() {
      this.input = true
      this.survey = false
    }
  }
}
</script>

<style>
* {
  margin: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
