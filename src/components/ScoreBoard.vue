<script setup>
import DropDown from './dropdown/dropdown.vue'
import { ref, onMounted } from 'vue'

const addedScore = ref(null);
const selectedOption = ref(null);
const options = ref([]);
const userScore = ref([]);

let socket = null;
onMounted(() => {
    fetchScores();
    // socket = new WebSocket('wss://lab6api.onrender.com/primus');
    socket = new WebSocket('ws://localhost:3000/primus');
    //listen for messages to websockets server (post)
    socket.onmessage = (event) => {
        let newScore = JSON.parse(event.data);
        if (newScore.action === "newScore") {
            //get all scores db
            emptyScores();
            fetchScores();
        }
    };
})

const fetchScores = async () => {
    //fetch scores from api
    // const response = await fetch('https://lab6api.onrender.com/');
    const response = await fetch('http://localhost:3001/');
    if (!response.ok) {
        console.error('Failed to fetch scores: ', response.statusText);
        return;
    }
    const fetchedScores = await response.json();
    // loop throug fetchscores and add to options
    fetchedScores.forEach((score) => {
        options.value.push(score.user);
    });
    // loop through fetchscores and add to userScore
    fetchedScores.forEach((score) => {
        userScore.value.push(score.score);
        console.log(score);
    });
}
const emptyScores = () => {
    options.value = [];
    userScore.value = [];
}

const handleSelection = (option) => {
    selectedOption.value = option;
}
const sendScore = () => {
    let newScore = {
        "score": addedScore.value,
        "user": selectedOption.value,
        "action": "newScore"
    }
    socket.send(JSON.stringify(newScore));
}
</script>

<template>
    <div class="dropdown-grid">
        <DropDown :options="options" @selected="handleSelection" />
        <input type="number" id="score" name="score" v-model="addedScore" placeholder="0">
        <button @click="sendScore()">Update Score for {{ selectedOption }}</button>
    </div>
    <div class="scores">
        <div class="score" v-for="(option, index) in options" :key="index">
            {{ option }}: {{userScore[index]}}
        </div>
    </div>
</template>

<style scoped>
.dropdown-grid {
    display: grid;
    grid-template-columns: 200px 200px 400px;
    grid-template-rows: 45px;
    grid-gap: 10px;
}

input {
    border-radius: 10px;
    border: none;
}

button {
    border-radius: 10px;
}
</style>
