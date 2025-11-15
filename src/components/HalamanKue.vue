<script setup>
import { ref, defineEmits } from 'vue';

const emit = defineEmits(['lanjut']);

const lilinMenyala = ref(true);
const tampilPesan = ref(false);

function tiupLilin() {
    if (lilinMenyala.value) {
        lilinMenyala.value = false;
        // Tunda penampilan pesan selamat ulang tahun (misalnya 500ms)
        setTimeout(() => {
            tampilPesan.value = true;
        }, 500);
        
        // Opsional: Mainkan suara tiupan/confetti
        // const audioTiup = new Audio('/tiup.mp3'); 
        // audioTiup.play();
    }
}
</script>

<template>
    <div class="halaman-kue-container">
        
        <div v-if="tampilPesan" class="pesan-ultah">
            <p>HOREEEEEE, 18 TAHUN!! <br> 👏👏👏</p>
        </div>

        <div class="birthday-cake-wrapper">
            <div class="birthday-cake">
                <div class="holder"></div>
                <div class="shadow"></div>
                <div class="cake">
                    <div class="chocolate"></div>
                </div>
                <div class="candle"></div>
                <div class="flame" :class="{ 'mati': !lilinMenyala }"></div>
            </div>
        </div>
        
        <button v-if="lilinMenyala" 
                @click="tiupLilin" 
                class="btn-interaksi btn-tiup">
            Tiup Lilin! 
        </button>

        <button v-else 
                @click="emit('lanjut')" 
                class="btn-interaksi btn-lanjut-akhir">
            Lanjut
        </button>
    </div>
</template>

<style scoped>
/* ------------------- CONTAINER UTAMA VUE ------------------- */
.halaman-kue-container { 
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
    width: 100vw;
    overflow: hidden;
}

/* ------------------- PESAN DAN TOMBOL ------------------- */
.pesan-ultah {
    position: absolute;
    top: 15%;
    color: #e91e63;
    font-size: 1.8em;
    font-weight: bold;
    text-align: center;
    padding: 20px;
    z-index: 10;
    animation: muncul 1s ease-out;
}
@keyframes muncul {
    from { opacity: 0; transform: translateY(-20px); }
    to { opacity: 1; transform: translateY(0); }
}

.btn-interaksi {
    padding: 10px 25px;
    border: none;
    border-radius: 20px;
    cursor: pointer;
    font-size: 1em;
    font-weight: bold;
    margin-top: 50px;
    transition: background-color 0.3s;
}

.btn-tiup {
    background-color: #e91e63;
    color: white;
}
.btn-lanjut-akhir {
    background-color: #e91e63;
    color: white;
}


/* ------------------- CSS KUE YANG DIKECILKAN ------------------- */
/* Wrapper untuk memusatkan kue dalam container Vue */
.birthday-cake-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    /* Tambahkan skala global agar mudah diatur responsif tanpa media query */
    transform: scale(1.1); /* Kecilkan 40% dari ukuran aslinya */
    margin-top: 150px;
}


.birthday-cake {
    position: relative;
    top: -60px;
    left: -21px;
}

.holder {
    position: absolute;
    width: 175px;
    height: 10px; 
    border-radius: 50px; 
    background-color: #212f3c;
    left: -70px;
    top: 5px; /* Dari 10px */
}

.holder:before {
    content: "";
    position: absolute;
    width: 50px;
    height: 75px; 
    background-color: #cacfd2;
    left: 65px;
    top: 10px; /* Dari 20px */
}

.holder:after {
    content: "";
    position: absolute;
    background-color:#F2AEBB; 
    width: 50px;
    height: 67.5px; 
    border-radius: 15px 15px 0 0; 
    left: 32.5px; 
    top: 10px; 
    box-shadow: 62.5px 0 #F2AEBB; /* Dari 125px */
}

.shadow {
    position: absolute;
    width: 175px; 
    height: 10px; 
    border-radius: 50%;
    top: 85px;
    left: -67.5px; 
    background-color: rgba(0,0,0,0.095);
    z-index: 1;
}

.shadow:before {
    content: "";
    position: absolute;
    background-color: #cacfd2;
    width: 75px; /* Dari 150px */
    height: 10px; /* Dari 20px */
    border-radius: 10px; /* Dari 20px */
    top: -5px; /* Dari -10px */
    left: 48px; /* Dari 96px */
}

.cake {
    position: absolute;
    overflow: hidden;
    width: 137.5px; /* Dari 275px */
    height: 70px; /* Dari 140px */
    background-color: #fad7a0;
    top: -65px; /* Dari -130px */
    left: -50.5px; /* Dari -101px */
    border-radius: 15px 15px 0 0; /* Dari 30px */
}

.cake:before {
    content: "";
    position: absolute;
    background-color: #935116;
    width: 137.5px;
    height: 7.5px; /* Dari 15px */
    top: 37.5px; /* Dari 75px */
    box-shadow: 0 15px #935116; /* Dari 30px */
}

.cake:after {
    content: "";
    position: absolute;
    background-color: #573612;
    width: 137.5px;
    height: 20px; 
    box-shadow: inset 0 5px rgba(0,0,0,0.07); /* Dari 10px */
}

.chocolate {
    position: absolute;
    background-color: #573612;
    width: 12.5px; /* Dari 25px */
    height: 22.5px; /* Dari 45px */
    top: 12.5px; /* Dari 25px */
    left: 0;
    z-index: 2;
    border-radius: 25px; /* Dari 50px */
    box-shadow: 
        12.5px 1px #fad7a0, 
        25px -5px #573612, 
        37.5px -2.5px #fad7a0, 
        50px 1.5px #573612, 
        62.5px 0 #fad7a0, 
        75px -2.5px #573612, 
        87.5px 1.5px #fad7a0, 
        100px -2.5px #573612, 
        112.5px -5px #fad7a0, 
        125px 2px #573612;
}

.candle {
    position: absolute;
    width: 7.5px; 
    height: 35px; 
    background-color: white;
    top: -100px; 
    left: 15px;
    box-shadow: inset 0 -5px #e74c3c; /* Dari -10px */
}

.candle:before {
    content: "";
    position: absolute;
    width: 7.5px;
    height: 7.5px; /* Dari 15px */
    background-color: #e74c3c;
    top: 5px; /* Dari 10px */
    box-shadow: 0 12.5px #e74c3c; /* Dari 25px */
}

.candle:after {
    content: "";
    position: absolute;
    width: 1px; 
    height: 7.5px;
    background-color: black;
    top: -7.5px;
    left: 3px; /* Dari 6px */
}

.flame {
    position: absolute;
    background-color: #f4d03f;
    opacity: 0.9;
    box-shadow: inset -2.5px 2.5px #d35400, 2.5px -2.5px 10px #fbffa3; 
    width: 10px; 
    height: 15px; 
    top: -120px; 
    border-radius: 50%;
    left: 13.25px;  
    animation: flame 0.5s infinite;
    transition: opacity 0.3s; /* Tambahkan transisi untuk efek tiup */
}

.flame.mati {
    opacity: 0;
    /* Anda bisa tambahkan transform/scale jika ingin efek api mengecil saat mati */
}

@keyframes flame {
    from, to { transform: scale(1,1); }
    25% { transform: scale(0.9,1.1); }
    50% { transform: scale(1.1,0.9); }
    75% { transform: scale(0.95,1.05); }
}
</style>