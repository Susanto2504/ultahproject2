<script setup>
import { defineProps, ref, onMounted } from 'vue';

const props = defineProps({
    audioCinta: Object // Menerima objek audio dari App.vue
});

const videoPlayer = ref(null);

function pauseAudio() {
    if (props.audioCinta && !props.audioCinta.paused) {
        props.audioCinta.pause();
        console.log("Lagu Cinta dihentikan sementara karena video diputar.");
    }
}

function playAudio() {
    if (props.audioCinta) {
        props.audioCinta.play().catch(e => console.error("Gagal melanjutkan Lagu Cinta:", e));
        console.log("Lagu Cinta dilanjutkan setelah video selesai.");
    }
}

function resumeAudio() {
    if (videoPlayer.value && videoPlayer.value.paused) {
        playAudio();
    }
}

onMounted(() => {
    if (props.audioCinta && props.audioCinta.paused) {
        playAudio();
    }
});

</script>

<template>
  <div class="halaman-penutup-container">
    <h1 class="judul-akhir">Ini Hadiah Terakhir dariku!</h1>
    
    <div class="video-akhir-wrapper">
    <video 
      ref="videoPlayer" 
      src="/penutup.mp4" 
      @play="pauseAudio" 
      @pause="resumeAudio"  @ended="playAudio" 
      controls
      class="video-player-akhir" >
      Browser Anda tidak mendukung tag video.
    </video>
  </div>
    
    <p class="pesan-cinta-akhir">
      "Terima kasih sudah menyelesaikan semuanya babykuuu. Kamu adalah hadiah terbaikku."
    </p>
    
    <a href="https://wa.me/+6281525866131?text=Terima%20Kasih%20Sayangku!%20Aku%20Kasih%20Bintang%20...." target="_blank" class="btn-chat">
      Chat Aku Sekarang! ❤️
    </a>
  </div>
</template>

<style scoped>
.halaman-penutup-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
     /* Pink sangat muda */
    padding: 20px;
    text-align: center;
}

.judul-akhir {
    font-size: 2em;
    color: #c2185b;
    margin-bottom: 25px;
}

.video-akhir-wrapper {
    width: 90%;
    max-width: 500px;
    margin-bottom: 30px;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
    border-radius: 15px;
    overflow: hidden;
}

.video-player-akhir {
    width: 100%;
    height: auto;
    display: block;
}

.pesan-cinta-akhir {
    font-style: italic;
    font-size: 1.2em;
    color: #4a2335;
    margin-bottom: 40px;
    max-width: 80%;
}

.btn-chat {
    padding: 15px 30px;
    background-color: #00e676; /* Hijau WhatsApp */
    color: white;
    text-decoration: none;
    border-radius: 30px;
    font-size: 1.1em;
    font-weight: bold;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    transition: background-color 0.3s;
}

.btn-chat:hover {
    background-color: #00c853;
}
</style>