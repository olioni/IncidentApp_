<template>
  <div id="main" :style="{backgroundColor: bgClr}">
    <div id="topBox">
      <div class="questionWrap" v-for="(question, index) in qObj" :key="question">
        <div class="questionBox" v-if="index === qIndex"  :id="index"><strong> {{ question.toUpperCase() }} </strong></div>
      </div>
    </div>
    <div id="bottomBox">

      <div class="responseWrap" v-for="(question, index) in qResponses" :key="index">
        <div class="responseContainer" v-if="index === qIndex" :id="index">
          <div class="response" :key="index" v-for="(items, index) in question" :id="index">
            <label class="switch">
              <input type="checkbox" @click="highlightRespo($event, index, question, qIndex)">
              <span class="slider"> 
                <h3 class="emoji">{{ qEmoji[qIndex][index] }}</h3>
                <h3>{{ items }}</h3>
              </span>
            </label>
          </div>
        </div>
      </div>


      <div class="submitWrap">
        <button class="submitButton" @click="nextQuestion(), changeBgColor()" v-if="submit">NEXT</button>
        <button class="submitButton" @click="displayEmailPopup()" v-if="finish">SUBMIT</button>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: 'surveyPopup',
  props: [],
  data() {
    return {
      qObj: {q1: 'What happened?', q2: 'What did you want to happen?\r\nOR Think was going to happen?', q3: 'Who has been affected or hurt?', q4: 'How?', q5: 'What needs to happen to fix things up?', q6: 'Next time?'},
      qResponses: {q1: ['Pushed', 'Hit', 'Kicked', 'Took Something', 'Bad/Nasty Words', 'Spat', 'Ignored', 'Upset Someone', 'Ran Away', 'Broke', 'Scratched', 'Made a Mess'], q2:['Angry', 'Go Away', 'Frustrated', "Don't Like/Don't Want To", 'Confused', 'Want To Play', 'Worried', 'Want To Go Home', 'Stop', 'Need Help', 'Not Fair'], q3: ['Student/Child', 'Teacher', 'School Helper', 'Teacher Aide', 'Whole Class', 'Grandparents', 'Bus Driver', 'Mum/Dad', 'Principal/DP'], q4: ['Hurt Head/Face', 'Hurt Arm', 'Hurt Leg', 'Broke Something', 'Made a Mess', 'Hurt Ears', 'Made Someone Scared', 'Made Someone Cry', 'Made Someone Embarassed', 'Made Someone Worried', "Hurt Someone's Feelings", 'Made Someone Sad'], q5: ['Give/Give Back', 'Say Something Nice', 'Check If Ok', 'Fix', 'Offer To', 'Help', 'Clean Up', 'Say I Will',]},
      qIndex: 'q1',
      qEmoji: {q1: ['✋', '🤛', '🦵', '🐱‍👤', '🤬', '💧', '🤐', '😠', '🏃‍♂️', '👋', '🤕', '🗑'], q2: ['😡', '🚶‍♂️', '😣', '🙄', '😟', '😢', '😧', '🙁', '🛑', '🙋‍♂️', '😔'], q3: ['👦', '👩‍🏫', '👨‍💼', '🙍‍♀️', '👥', '👴', '🚌', '👪', '👨‍💼'], q4: ['🤕', '💪', '🦵', '👋', '🗑', '👂', '😰', '😭', '😖', '😞', '😟', '😢'], q5: ['🤲', '🗨', '🤔', '🛠', '🤗', '👏', '💬', '🙏']},
      selectedResponses: {
        q1: [],
        q2: [],
        q3: [],
        q4: [],
        q5: []
      },

      qInt: 1,

      buttonText: 'NEXT',
      submit: true,
      finish: false,

      colorArr: ['#8f02e0', '#e0025f', '#37db81', '#e37100', '#ad0c0c'],
      bgClr: '#1e6ae3',

      x: 0
    }
  },
  methods: {
    // On click of NEXT button, change the colour of the background.
    // Pick from array of colours (colorArr) and once colour is picked, splice out the chosen colour
    // so there are no repeats in the background
    changeBgColor() {
      let rng = Math.floor(Math.random() * this.colorArr.length)
      this.bgClr = this.colorArr[rng]
      this.colorArr.splice(rng, 1)
    },
    // Emit to App.vue to display the emailPopup
    displayEmailPopup() {
      this.$emit('showPopup')
      this.$emit('responses', this.selectedResponses)
    },
    // Change questions via qIndex
    // Change button if the person has reached question 5
    nextQuestion() {
      this.qInt++
      this.qIndex = 'q' + this.qInt

      if (this.qIndex == 'q5') {
        this.submit = false
        this.finish = true
      }

    },
    // Once response is clicked, add to object that holds the question as the key and the array of responses as values
    highlightRespo(event, id, question, qIndex) {
      if (event.target.checked == true) {
        this.selectedResponses[qIndex].push(question[id])
      } else {
        let removeIndex = this.selectedResponses[qIndex].indexOf(question[id])
        this.selectedResponses[qIndex].splice(removeIndex, 1)
      }
      console.log(this.selectedResponses)
    },
  },
  mounted() {
    
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
  margin: 0;
}

#main {
  background-color: #1e6ae3;
  
  display: flex;
  width: 100vw;
  height: 100vh;

  justify-content: center;
  align-items: center;

  flex-direction: column;

  transition: 0.6s;
}

#topBox {
  height: 22vh;
  width: 100vw;

  display: flex;
  justify-content: center;
  align-items: center;
}

#bottomBox {
  height: 70vh;
  width: 100vw;

  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: row;
  flex-wrap: wrap;
}

h3 {
  color: white;
}

.questionWrap {
  display: flex;
  
  justify-content: center;
  align-items: center;
  
}

.questionBox {
  height: 20vh;
  width: 40vw;

  background-color: white;

  display: flex;
  justify-content: center;
  align-items: center;

  border-radius: 15px;

  font-size: 18px;
  color: black;
}

.responseWrap {
  display: flex;
  justify-content: center;
  align-items: center;
}

.responseContainer {
  width: 100%;
  display: flex;

  justify-content: center;
  align-items: center;

  flex-wrap: wrap;  
}

.response {
  /* background-color: white; */
  
  width: 15vw;
  height: 21vh;

  border-radius: 10px;

  display: flex;
  justify-content: center;
  align-items: center;

  flex-wrap: wrap;
  text-overflow: hidden;

  transition: 0.3s;

  /* margin-top: 4px;
  margin-right: 4px;
  margin-left: 4px;
  margin-bottom: 4px; */

  flex-direction: column  ;
}

.response:hover{
  cursor: pointer;
  font-size: 18px;
}

.submitButton {
  width: 80vw;
  height: 10vh;
  
  background-color: white;

  border: none;
  border-radius: 10px;
  
  transition: 0.3s;
}

.submitButton:hover {
  cursor: pointer;
  font-size: 20px;

  background-color: rgb(238, 238, 238);
}

.response h3 {
  color: black;
}

.emoji {
  font-size: 30px;
  transition: 0.3s;
}

.switch {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 90%;
  height: 90%;
}

.switch input { 
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  /* position: absolute; */
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;

  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  flex-direction: column;
  border-radius: 5px;
}


input:checked + .slider {
  background-color: #55eb34;
}

input:focus + .slider {
  box-shadow: 0 0 1px #2196F3;
}

@media only screen and (max-width: 600px) {
  .response {
    height: 16vh;
    width: 20vw;
    font-size: 17px;
  }

  .response h3 {
    font-size: 16px;
  }

  .responseWrap {
    height: 60vh;
  }

  .responseContainer {
    height: 51vh;
    width: 90vw;
    display: flex;
  }

  .questionBox {
    width: 80vw;
    font-size: 30px;
  }

  .submitButton {
    height: 10vh;
    font-size: 30px;
  }

  .slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;

  display: flex;
  justify-content: center;
  align-items: center;
  /* width: 20vw; */
  /* height: 15vh; */
  flex-direction: column;
  border-radius: 5px;
  }

}
</style>
