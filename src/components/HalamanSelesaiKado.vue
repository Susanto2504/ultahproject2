<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
    kadoAktif: Number,
    audioCinta: Object,
    pesanCinta: String,
    videoSource: String
});

const emit = defineEmits(['selesai']);

function selesai() {
    const videoEl = document.querySelector('.video-player');
    if (videoEl && !videoEl.paused) {
        videoEl.pause();
    }
    emit('selesai');
}

function handleVideoPlay() {
    // Video Play -> Lagu Cinta PAUSE
    if (props.audioCinta && !props.audioCinta.paused) {
        props.audioCinta.pause();
        console.log("Video Kado (Halaman 13) diputar: Lagu Cinta PAUSE.");
    }
}

function handleVideoPause() {
    // Video PAUSE -> Lagu Cinta PLAY
    if (props.audioCinta && props.audioCinta.paused) {
        props.audioCinta.play().catch(e => console.error("Gagal melanjutkan Lagu Cinta saat video di-pause di Halaman 13:", e));
        console.log("Video Kado (Halaman 13) Pause: Lagu Cinta PLAY.");
    }
}

function handleVideoEnd() {
    // Biarkan video selesai. Logika play audio tidak selalu perlu di sini
    // karena video selesai != tombol Selesai diklik.
    console.log("Video Kado (Halaman 13) selesai diputar.");
}
</script>

<template>
    <div class="halaman-selesai-kado-container">
        <div class="video-kado-wrapper">
            <video 
                class="video-player" 
                :src="props.videoSource" 
                controls 
                @play="handleVideoPlay"
                @pause="handleVideoPause"
                @ended="handleVideoEnd">
            </video>
        </div>
        
        <p class="pesan-cinta-teks">"{{ pesanCinta }}"</p> 
        
        <button class="btn-selesai" @click="selesai">
            SELESAI
        </button>
    </div>
</template>

<style scoped>
.halaman-selesai-kado-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    background-color: #f7a0c1;
    padding: 20px;
}

.video-kado-wrapper {
    width: 90%;
    max-width: 450px;
    height: auto;
    background-color: white;
    border-radius: 15px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    margin-bottom: 30px;
    overflow: hidden;
}

.video-player {
    width: 100%;
    height: auto;
    display: block;
}

.pesan-cinta-teks {
    font-style: italic;
    font-size: 1.1em;
    color: #4a2335; 
    margin: 10px 0 20px 0;
    max-width: 80%;
    text-align: center;
    line-height: 1.5;
}

.info-kado {
    font-size: 0.9em;
    color: #a04052;
    margin-bottom: 30px;
    font-weight: bold;
}

.btn-selesai {
    padding: 15px 40px;
    background-color: #a04052;
    color: white;
    border: none;
    border-radius: 30px;
    cursor: pointer;
    font-size: 1.1em;
    font-weight: bold;
    transition: background-color 0.3s;
}

.btn-selesai:hover {
    background-color: #7b2938;
}
</style>