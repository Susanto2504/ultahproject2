<script setup>
import { ref, defineEmits, defineProps} from 'vue';

const emit = defineEmits(['lanjut']);

const props = defineProps({
    audioCinta: Object // Harap pastikan Anda mengirimkan audioCinta dari App.vue
});
const videoSelesai = ref(false);
const lastKnownTime = ref(0);
let isSeeking = false;


function lanjut() {
    // Hanya panggil emit('lanjut') jika video sudah selesai
    if (videoSelesai.value) {
        emit('lanjut');
    }
}

function handleVideoPlay() {
    // 1. Logika: Hentikan Lagu Cinta saat video mulai diputar
    if (props.audioCinta && !props.audioCinta.paused) {
        props.audioCinta.pause();
        // Kita tidak perlu mengatur currentTime = 0 agar bisa lanjut di halaman berikutnya
        console.log("Video diputar, Lagu Cinta dihentikan sementara.");
    }
}

function handleVideoPause() {
    // Aksi: Video PAUSE -> Lagu Cinta PLAY
    // Logika ini dipanggil ketika pengguna menekan tombol pause di video player.
    if (props.audioCinta && props.audioCinta.paused) {
        props.audioCinta.play().catch(e => console.error("Gagal melanjutkan Lagu Cinta saat video di-pause:", e));
        console.log("Video Pause: Lagu Cinta PLAY.");
    }
}

function handleVideoSeeking(event) {
    const videoElement = event.target;

    // Periksa apakah pengguna mencoba melompat ke depan
    // Membandingkan currentTime dengan waktu terakhir yang diketahui (lastKnownTime)
    if (videoElement.currentTime > lastKnownTime.value) {
        
        // Mencegah seek loop yang tak terbatas
        if (!isSeeking) {
            isSeeking = true;
            console.log(`Mencoba skip dari ${lastKnownTime.value.toFixed(2)}s ke ${videoElement.currentTime.toFixed(2)}s.`);

            // Kembalikan video ke waktu terakhir yang diketahui
            videoElement.currentTime = lastKnownTime.value; 
        }
        
        isSeeking = false;
    }
}

// Perlu mengupdate lastKnownTime setiap 1 detik
function updateLastKnownTime(event) {
    const videoElement = event.target;
    
    // Hanya update jika video sedang diputar dan tidak sedang seeking
    if (!videoElement.paused && !videoElement.seeking) {
        lastKnownTime.value = videoElement.currentTime;
    }
}

// Dipanggil saat video di-pause atau selesai (ended)
function handleVideoEnd(event) {
    const videoElement = event.target;

    if (videoElement.ended) {
        // Aktifkan tombol LANJUT
        videoSelesai.value = true;
        videoElement.controls = true;
        // JANGAN melanjutkan Lagu Cinta di sini. Biarkan Lagu Cinta dihentikan
        // hingga transisi ke Halaman 10. Logika melanjutkan lagu harus ada di watch(halamanAktif) App.vue
        console.log("Video selesai. Tombol Lanjut aktif. Lagu Cinta tetap PAUSED.");
        
    } 
}

</script>

<template>
  <div class="halaman-video-final-container">
    
    <h1 class="judul-instruksi">LIHAT YA VIDEO INSTRUKSI INI!</h1>
    
    <div class="video-container">
      <video class="video-player" src="/大家好！ (2).mp4" controls
                @play="handleVideoPlay"
                @pause="handleVideoPause"
                @ended="handleVideoEnd"
                @seeking="handleVideoSeeking"
                @timeupdate="updateLastKnownTime">
            </video>
    </div>
    
    <button 
      class="btn-mulai-final" 
      :class="{ 'aktif': videoSelesai }"
      :disabled="!videoSelesai" 
      @click="lanjut">
      MULAI
    </button>

  </div>
</template>

<style scoped>
.halaman-video-final-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    width: 100%;
    padding: 20px;
    text-align: center;
    box-sizing: border-box;
}

.judul-instruksi {
    color: #e91e63; /* Warna merah gelap dari desain */
    font-size: 1.3em;
    font-weight: bold;
    margin-bottom: 30px;
    font-family: 'Georgia', serif; /* Sesuaikan font jika Anda punya custom font */
}

.video-container {
    width: 90%;
    width: 450px;
    height: 250px; 
    background-color: white;
    border-radius: 15px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
    margin-bottom: 40px;
}

.video-player {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.btn-mulai-final {
    padding: 15px 40px;
    background-color: #e91e63; /* Warna merah tua dari desain */
    color: white;
    border: none;
    border-radius: 30px;
    cursor: not-allowed; /* Kursor default non-aktif */
    font-size: 1.1em;
    font-weight: bold;
    transition: all 0.3s;
    opacity: 0.5; /* Opasitas rendah saat non-aktif */
}

.btn-mulai-final.aktif {
    opacity: 1;
    cursor: pointer;
    background-color: #e91e63; /* Beri warna lebih cerah saat aktif */
}

.btn-mulai-final:disabled {
    cursor: not-allowed;
}
</style>