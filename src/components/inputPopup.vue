<template>

<div class="container" :style="{backgroundColor: bgClr}">
    <div class="mainWrap">
        <div class="questionWrap">
            <div class="primaryQuestionWrap">
                <h2 class="question"> {{ question[qIndex] }}</h2>
            </div>

            <div class="textAreaDiv">
                <textarea class="textarea" ></textarea>
            </div>
        </div>

        <div class="questionWrap">
            <div class="secondaryQuestionWrap">
                <h2 class="question"> {{ question[qIndex + 1] }} </h2>
            </div>

            <div class="textAreaDiv">
                <textarea class="textarea" :id="ques"></textarea>
            </div>
        </div>
        <button @click="nextQuestion()" v-if="next" class="submitButton">NEXT</button>
        <button @click="displayEmailPopup()" v-if="submit" class="submitButton">SUBMIT</button>
    </div>
</div>

</template>

<script>
export default {
    name: 'inputPopup',
    props: [],
    data() {
        return {
            qInt: 2,
            qIndex: 0,
            question: ['What happened? What was your part in it?', 'How were you feeling when it happened?', 'What were you thinking/ What did you think would happen next? What did you want to happen?', 'Who do you think has been affected? In what way?', 'What has been the worst thing about it?', 'What is needed to make things right?', 'How can we make sure it doesn’t happen again?', 'What help / follow-up do you need to make sure the problem is solved?'],
        
            next: true,
            submit: false,

            bgClr: '#42a7f5',


        }
    },
    methods: {
        nextQuestion() {
            this.qIndex = this.qIndex + this.qInt
            
            if (this.qIndex == 6) {
                this.next = false
                this.submit = true
            }
        },
        displayEmailPopup() {
            this.$emit('showPopup')
        }
    }
}

</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');

* {
    margin: 0;
}

.container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    height: 100vh;
    width: 100vw;
}

.mainWrap {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
}

.textarea {
    resize: none;
    width: 60vw;
    height: 30vh;
    border-radius: 10px;
    font-family: 'Roboto', sans-serif;
    font-size: 18px;
}

.questionWrap {
    height: 30vw;
}

.question {
    height: 5vh;
    color: black;
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

</style>