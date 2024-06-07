<template>
    <div class="container d-flex justify-content-center">
        <div class="game-container">
            <div class="d-flex flex-row justify-content-between pt-2">
                <button class="btn" @click="jumbleLetters">
                <ShuffleIcon/>
                </button>
                <div class="seconds-text">
                    <div>{{ seconds == 60 ? "01:00" : seconds > 9 ? "00:" + seconds : "00:0" + seconds }}</div>
                </div>
            </div>
            <div class="text-center mt-5">
                <DisplayScore :currentScore="String(score)" :currentWords="String(wordCount)"/>
            </div>

            <div class="text-center">
                <button class="btn enter-button" @click="checkWord" :disabled="!(inputs.length > 2) || !playing">Enter</button>
            </div>
            
            <div class="p-2">
                <div>
                    <DisplayInputComponent :inputs="inputs" :handleRemoveLetter="handleRemoveLetter"/>
                </div>
                
                <div class="mt-3">
                    <InputComponent :letters="letters" :handleInput="handleInput" :usedInputs="usedInputs" :isPlaying="playing"/>
                </div>
                <div class="game-controls d-flex justify-content-center mt-3">
                    <button class="btn btn-warning" @click="startGame" :disabled="playing">Start game</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
// imports
import { ref } from 'vue';
import DisplayScore from '../components/DisplayScore.vue';
import ShuffleIcon from '../components/ShuffleIcon.vue';
import DisplayInputComponent from '../components/DisplayInputComponent.vue';
import InputComponent from '../components/InputComponent.vue';
import words from '../assets/words_dictionary.json';

// ref constants
const letters = ref<string[]>(['L', 'E', 'I', 'N', 'G', 'R']);
const inputs = ref<string[]>([]);
const usedInputs = ref<number[]>([]);
const score = ref(0);
const wordCount = ref(0);
const knownWords = ref<string[]>([]);
const seconds = ref(0);

const timer = ref<NodeJS.Timeout>();
const playing = ref(false);

const PLAYING_TIME = 60;

// logic functions

// initialise game
const initGame = () => {
    // set game values to zero
    score.value = 0;
    wordCount.value = 0;
    knownWords.value = [];
    usedInputs.value = [];
    seconds.value = 0;
    // generate random letters
    letters.value = generateLetters();
}

const startGame = () => {
    initGame();
    playing.value = true;
    startTimer();
}

const startTimer = () => {
    clearInterval(timer.value);
    timer.value = setInterval(() => {
        seconds.value += 1;
        if(seconds.value >= PLAYING_TIME) {
            endGame();
        }
    }, 1000);
}

const endGame = () => {
    playing.value = false;
    usedInputs.value = [];
    inputs.value = [];
    stopTimer();
}

const stopTimer = () => {
    clearInterval(timer.value);
}

// update timer

// generate letters
const generateLetters = () => {
    const characters = [];
    for (let i = 0; i < 6; i++) {
    // Generate a random character code between 'a' (97) and 'z' (122)
    const charCode = Math.floor(Math.random() * 26) + 97;
    characters.push(String.fromCharCode(charCode).toUpperCase());
    }
    return characters;
}

// handle input
const handleInput = (inputLetter: string, index: number) => {
    if(inputs.value.length > 5 || (usedInputs.value.includes(index))){
        return;
    }
    // add inputLetter to inputs
    inputs.value.push(inputLetter);
    // mark index as used
    usedInputs.value.push(index);
}

// handle resetting a letter
const handleRemoveLetter = (index: number) => {
    console.log(inputs.value.length)
    if(index>=0) {
        inputs.value.splice(index, 1);
        usedInputs.value.splice(index, 1);
    }    
}

// checks validity of word and gives score
const checkWord = () => {
    var wordToCheck = inputs.value.join('').toLowerCase();
    
    if(wordToCheck in words && !knownWords.value.includes(wordToCheck)){
        switch(inputs.value.length){
            case 3:
                score.value += 300;
                break;
            case 4:
                score.value += 800;
                break;
            case 5:
                score.value += 1000;
                break;
            case 6:
                score.value += 2000;
                break;
        }
        wordCount.value += 1;
        knownWords.value.push(wordToCheck);
        resetValues();
    }
}

// jumbles order of the letters
const jumbleLetters = () => {
    resetValues();
    const letterCopy = [...letters.value];
    for (let i = letterCopy.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [letterCopy[i], letterCopy[j]] = [letterCopy[j], letterCopy[i]]; // Swap elements
    }
    letters.value = letterCopy;
}

// resets entered letters and marked indexes
const resetValues = () => {
    inputs.value = []; 
    usedInputs.value = [];
}
</script>

<style scoped>
.game-container {
    margin-top: 5rem;
    width: 40rem;
}
.enter-button {
    margin: 3rem;
    width: 20rem;
    background-color: rgb(158, 120, 219);
}
.enter-button:hover {
    background-color: rgb(137, 104, 190);
}
.enter-button:disabled {
    background-color: rgb(137, 104, 190);
}
.seconds-text {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 1.4rem;
    padding: 0.4rem;
    color: white;
    background-color: rgb(107, 67, 145);
    border-radius: 0.5rem;
}
</style>