<template>
    <div class="container">
        <button class="circle" @click="beginApp()">
            START
        </button>
    </div>
</template>

<script>
export default {
    name: 'startPopup',
    props: [],
    data() {
        return {

        }
    },
    methods: {
        // On start button click, emit to App.vue and switch to surveyPopup
        beginApp() {
            this.$emit('showApp')
        }
    }
}
</script>

<style scoped>

* {
    margin: 0;
}

.container {
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #42a7f5;
}

.circle {
    width: 45vw;
    height: 80vh;
    border-radius: 50%;
    font-size: 50px;
    border: none;
    background-color: white;
    transition: 0.3s;
    box-shadow: 0 18px rgb(197, 197, 197);
}

.circle:hover {
    cursor: pointer;
    font-size: 60px;
    background-color: rgb(233, 233, 233)
}

.circle:active {
    transform: translateY(4px);
    box-shadow: 0 12px rgb(197, 197, 197);
}

@media only screen and (max-width: 600px) {
    .circle {
        width: 90vw;
        height: 55vh;
    }
}

@media only screen and (max-width: 500px) {
    .circle {
        width: 90vw;
        height: 45vh;
    }
}

@media only screen and (max-width: 400px) {
    .circle {
        width: 90vw;
        height: 40vh;
    }
}

@media only screen and (max-width: 300px) {
    .circle {
        height: 25vh;
    }
}
/* 
@media only screen and (max-width: 700px) {
    .circle {
        width: 82vw;
        height: 70vh;
    }
} */

</style>