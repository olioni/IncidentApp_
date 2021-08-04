<template>
    <div class="container" :style="{backgroundColor: bgClr}">
            <div v-if="confirmed" class="tyWrap">
                <h2 class="tyText" @mouseover="hover()" @mouseout="unhover()"> {{ tyText }} </h2>
            </div>
            <form class="contact-form">
                <div v-if="confirming" class="inputsWrap">
                    <div class="warning" v-if="warning">
                        <h2>PLEASE ENTER ALL DETAILS INTO THE FORM</h2>
                    </div>
                    <div class="inputContainer">
                        <h2 class="inputHeader">STUDENT NAME</h2>
                        <input type="email" v-model="emailBody.student_name">
                    </div>

                    <div class="inputContainer splitContainer">
                        <div>
                        <h2 class="inputHeader">PARENT NAME</h2>
                        <input type="email" v-model="emailBody.parent_name">
                        </div>
                        <div>
                        <h2 class="inputHeader">PARENT EMAIL ADDRESS</h2>
                        <input type="email" v-model="emailBody.parent_email" :class="[isEmailValid()]">
                        </div>
                    </div>
                    
                    <div class="inputContainer splitContainer">
                        <div>
                        <h2 class="inputHeader">TEACHER NAME</h2>
                        <input type="email" v-model="emailBody.teacher_name">
                        </div>
                        <div>
                        <h2 class="inputHeader">TEACHER EMAIL ADDRESS</h2>
                        <input type="email" v-model="emailBody.teacher_email" :class="[isEmailValid()]">
                        </div>
                    </div>


                </div>
                <div v-if="confirming" class="buttonWrap">
                    <input class="confirmButton" type="submit" value="submit" @click="confirmEmails(), rng(), sendEmail()" :style="{textTransform: toUpperCase}" :disabled="disabled">
                </div>
            </form>
    </div>
</template>

<script>
import emailjs  from 'emailjs-com';

export default ({
    name: 'emailPopup',
    props: ['responsesFromApp'],
    data() {
        return {
            confirming: true,
            confirmed: false,

            bgClr: '#7c4ebf',
            tyText: 'Thank you for your input 👍',

            upperCase: 'uppercase',

            emailBody: {
                parent_name: '',
                parent_email: '',
                teacher_name: '',
                teacher_email: '',

                student_name: '',

                response: this.responsesFromApp
            },

            toUpperCase: 'uppercase',
            warning: false,
            disabled: true,

            reg: /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,24}))$/,
            btnColor: 'white'
        }
    },
    methods: {
        // Change displays from email with inputs to thank you page
        confirmEmails() {
            this.confirming = false
            this.confirmed = true
        },
        // Set the background colour in the beginning of loading the page
        rng() {
            this.bgClr = '#5bc7ac'
        },
        // Upon hovering the thank you text, change the text
        hover() {
            this.tyText = '✨ Thank you for your input ✨'
        },
        // Upon unhovering over the text change the text back to what it was prior
        unhover() {
            this.tyText = 'Thank you for your input 👍'
        },
        // Once inputs are filled send emails to users via emailjs, sends email to student and teacher
        sendEmail() {
            console.log('sending email')
            console.log(this.emailBody)
            emailjs.send('service_6y6k81s', 'template_2h8mbw5', this.emailBody, 'user_9ldq54a73yTROgi8azUq4')
                .then((result) => {
                    console.log('SUCCESS PARENT!', result.status, result.text);
                }, (error) => {
                    console.log('FAILED...', error);
                });
            emailjs.send('service_6y6k81s', 'template_5tnfcgk', this.emailBody, 'user_9ldq54a73yTROgi8azUq4')
                .then((result) => {
                    console.log('SUCCESS! TEACHER', result.status, result.text);
                }, (error) => {
                    console.log('FAILED...', error);
                });
        },
        isEmailValid() {
            return (this.emailBody.parent_email == '' && this.emailBody.teacher_email == '') ? '' : (this.reg.test(this.emailBody.parent_email) && this.reg.test(this.emailBody.teacher_email)) ? this.disabled = false : this.disabled = true ;
        },

    }
})
</script>

<style scoped>

* {
    margin: 0;
}

.container {
    display: flex;
    position: absolute;

    width: 100vw;
    height: 100vh;

    justify-content: center;
    align-items: center;

    background-color: #1e6ae3;

    flex-direction: column;

    transition: 0.6s;
}

.inputsWrap {
    width: 100vw;
    height: 70vh;

    transition: 0.6s;

    /* border: red 1px solid; */
}

.inputContainer {
    height: 13vh;
}

.splitContainer {
    height: 24vh;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: flex-end;
    /* border: yellow 1px solid; */
}

.splitContainer input {
    width: 40vw;
    margin: 20px;
}

.inputHeader {
    margin-bottom: 2vh;
    color: black;
}

.confirmButton {
  width: 80vw;
  height: 10vh;
  
  background-color: white;

  border: none;
  border-radius: 10px;
  
  transition: 0.3s;
}

.confirmButton:hover {
  cursor: pointer;
  font-size: 20px;

  background-color: rgb(238, 238, 238);
}

input {
    width: 60vw;
    height: 6vh;

    border: none;

    border-radius: 5px;

    font-size: 18px;

    text-align: center;
}

.tyWrap {
    width: 100vw;
    height: 100vh;
    
    display: flex;
    justify-content: center;
    align-items: center;

    color: white;

    font-size: 25px;

    transition: 0.6s;
}

.tyText {
    transition: 0.3s;
}
</style>