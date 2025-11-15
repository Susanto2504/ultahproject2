<script setup>
import { ref, onMounted, defineEmits, onBeforeUnmount } from 'vue';

const emit = defineEmits(['login-sukses']);

const usernameInput = ref('');
const passwordInput = ref('');
const errorMessage = ref(false); 
const teksTampilan = ref('');

const VALID_USERNAME = "Novianti Duapadang";
const VALID_PASSWORD = "271107";
const ISITEKS = "Happy Birthday Baby!";

const audioUltah = ref(null);
const putarAudio = ref(false); 

let textIndex = 0;
let timerTyping = null;
let timerReset = null;

function type() {
    if (textIndex < ISITEKS.length) {
        teksTampilan.value += ISITEKS.charAt(textIndex);
        textIndex++;
        timerTyping = setTimeout(type, 85); 
    } else {
        timerReset = setTimeout(resetAndType, 1500);
    }
}

function resetAndType() {
    teksTampilan.value = "";
    textIndex = 0;
    timerTyping = setTimeout(type, 585);
}

function handleLogin() {
    if (usernameInput.value === VALID_USERNAME && passwordInput.value === VALID_PASSWORD) {
        errorMessage.value = false;
        
        if (audioUltah.value && !audioUltah.value.paused) {
            audioUltah.value.pause();
        }
        emit('login-sukses');

        usernameInput.value = '';
        passwordInput.value = '';

    } else {
        errorMessage.value = true;
        usernameInput.value = '';
        passwordInput.value = '';
    }
}

function putarlagu() {
    if (putarAudio.value || !audioUltah.value) return;

    audioUltah.value.play()
        .then(() => {
            putarAudio.value = true;
            document.body.classList.add('audio-playing'); 
        })
        .catch(error => {
            console.error("Gagal memutar audio:", error);
        });
}

onMounted(() => {
    setTimeout(type, 1000); 

    const audioEl = document.getElementById('myaudio');
    if (audioEl) {
        audioUltah.value = audioEl;
    }

});

onBeforeUnmount(() => {
    clearTimeout(timerTyping);
    clearTimeout(timerReset);
});
</script>

<template>
  <div class="halaman-utama-vue" @click.once="putarlagu">
    
    <div id="teks-container">
        <span id="text">{{ teksTampilan }}</span>
    </div>

    <div class="login-box">
        <form @submit.prevent="handleLogin"> 
            
            <div class="input-group">
                <label for="username">Username</label>
                <input type="text" id="username" v-model="usernameInput" required>
            </div>

            <div class="input-group">
                <label for="password">Password</label>
                <input type="password" id="password" v-model="passwordInput" required>
            </div>

            <div id="error-message" v-if="errorMessage">
                Username atau Password Kamu salah.
            </div>

            <button type="submit">Masuk</button>
        </form>
    </div>

    <audio src="/happy-birthday-254480.mp3" id="myaudio" loop></audio>
  </div>
</template>

<style scoped>
.halaman-utama-vue { 
    
    display: flex;
    gap: 30px;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    cursor: pointer;
}

.halaman-utama-vue.audio-playing {
    cursor: default;
}

#teks-container {
    min-width: 21ch;
    width: 80%;
    text-align: center;
    height: 2.2rem;
}

#text {
    color: white;
    animation: blink-caret 0.75s step-end infinite;
    font-size: 1.28rem;
    font-weight: bold;
}

.login-box {
    background-color: white;
    padding: 30px;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    width: 70%;
}

.input-group {
    margin-bottom: 15px;
}

.input-group label {
    display: block;
    margin-bottom: 5px;
    font-weight: bold;
}

.input-group input {
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 10px;
    box-sizing: border-box;
}

button {
    width: 100%;
    padding: 10px;
    background-color: rgb(218, 47, 175);
    color: white;
    font-size: 1rem;
    border: none;
    cursor: pointer;
    border-radius: 10px;
    transition: background-color .3s;
}

button:hover {
    background-color: #000000;
}

#error-message {
    color: red;
    font-size: 0.8em;
    font-style: italic;
    margin-top: 10px;
}
</style>