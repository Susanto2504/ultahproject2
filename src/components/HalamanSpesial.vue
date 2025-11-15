<script setup>
import { defineProps, defineEmits } from 'vue';

import TombolSudah from './TombolSudah.vue';
import TombolBelum from './TombolBelum.vue';

const props = defineProps({
    halaman: Number,
    audioCinta: Object, // Prop yang menerima objek audio dari App.vue
});

const emit = defineEmits(['next', 'prev', 'selesai', 'kembali', 'lanjut', 'kembaliVideo']); 

// --- Logika Kontrol Audio-Video ---

function handleVideoPlay() {
    if (props.audioCinta && !props.audioCinta.paused) {
        props.audioCinta.pause();
        console.log("Video diputar, Lagu Cinta dihentikan.");
    }
}

function handleVideoStop() {
    if (props.audioCinta && props.audioCinta.paused) {
        props.audioCinta.play().catch(e => console.error("Gagal melanjutkan Lagu Cinta:", e));
        console.log("Video berhenti/selesai, Lagu Cinta dilanjutkan.");
    }
}

function startAudioOnInteraction() {
    if (props.audioCinta && props.audioCinta.paused) {
        props.audioCinta.play().catch(e => console.error("Gagal memulai Lagu Cinta:", e));
    }
}
</script>

<template>
    <div class="halaman-spesial">
        
        <div v-if="props.halaman === 2" class="kartu-ucapan" @click.once="startAudioOnInteraction">
            <div class="bungkus-ucapan">
                <h2 class="judul-kartu">KARTU UCAPAN</h2>
                <p class="ucapan-teks">
                    Happy birthday, Sayang! hari ini aku sangat berterima kasih sama Tuhan karena ini adalah ulang tahunmu yang ketiga kalinya yang kita rayakan bersama. Sejauh apapun masalah yang telah kita hadapi, aku masih bisa merayakan ulang tahunmu yang ke 18 tahun. Aku ndak bisa banyak menuliskan kata-kata sayang, karena aku tidak pandai berkata-kata. Panjang umur ya sayang, doaku selalu yang terbaik untukmu! 👼👼
                </p>
                
                <div class="btn">
                    <button @click="emit('next')" class="btn-video">Lihat Video</button>
                </div>
            </div>
            
            
        </div>

        <div v-else-if="props.halaman === 3" class="halaman-video">
            <h2 class="judul-video">Ditonton Sampai Habis Ya!!</h2>

            <div class="video-container">
                <video 
                    src="/lv_0_20251019024825.mp4" 
                    controls
                    @play="handleVideoPlay"
                    @pause="handleVideoStop"
                    @ended="handleVideoStop"
                >
                    Browser Anda tidak mendukung tag video.
                </video>
            </div>

            <p class="konfirmasi">Sudah Nonton Videonya?</p>
            <div class="tombol-konfirmasi">
                <button class="btn-konfirmasi" @click="emit('prev')">Belum</button>
                <button class="btn-konfirmasi" @click="emit('selesai')">Sudah</button>
            </div>
        </div>

        <TombolSudah v-else-if="props.halaman === 5" 
            @lanjut="emit('lanjut')" 
            @kembaliVideo="emit('kembaliVideo')"
            /> 
        <TombolBelum v-else-if="props.halaman === 4" 
            @kembali="emit('kembali')" 
            />
    </div>

    
</template>

<style scoped>
.halaman-spesial {
    height: 100vh;
    width: 100vw; 
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 30px;
    overflow-x: hidden; 
    position: relative; 
}

/* Container untuk kartu ucapan dan gambar */
.kartu-ucapan {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center; 
    height: 100vh; 
    position: relative; 
    max-width: 100%; 
    overflow: hidden; /* Sembunyikan semua yang meluap dari kartu ucapan */
}

.bungkus-ucapan {
    max-width: 400px;
    background-color: #f5d3c44b;
    padding-top: 50px; 
    padding-bottom: 50px; 
    width: 75vw;
    padding-left: 30px;
    padding-right: 30px;
    border-radius: 10px;
    box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
    text-align: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start; 
    cursor: pointer; 
    position: relative;
    z-index: 5;
}

.gambar {
    position: absolute; 
    /* ✅ Naikkan gambar: Geser gambar ke atas (bottom positif) */
    bottom: 15%; 
    /* ✅ Tarik gambar ke kanan (sedikit masuk) */
    right: -5%; 
    width: 40%; 
    max-width: 18em; 
    z-index: 10; 
}

.gambar img {
    width: 100%;
    height: auto;
    display: block;
}

.judul-kartu {
    color: #e91e63;
    font-weight: bold;
    font-size: 2em;
    margin-bottom: 20px;
}

.ucapan-teks {
    font-size: 1.2em;
    margin-bottom: 20px; 
    color: #ffffff;
    line-height: 1.5;
    flex-grow: 1; 
    display: flex;
    align-items: center; 
    justify-content: center;
}

.btn {
    width: 100%;
    padding-top: 5px; 
}

.btn-video {
    padding: 10px 20px;
    background-color: #e91e63;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    margin-top: 15px;
    margin-bottom: 0; 
    font-size: 1.1em;
}

/* --- Halaman Video (halamanAktif === 3) --- */
.halaman-video {
    max-width: 400px;
    padding: 30px;
    border-radius: 10px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    text-align: center;
    cursor: pointer; 
}

.judul-video {
    color: #e91e63;
    font-weight: bold;
    font-size: 3em;
    margin-bottom: 20px;
}

.video-container video {
    width: 100%;
    max-height: 300px;
    border-radius: 5px;
    padding: 5px;
    background-color: rgb(255, 255, 255);
    margin: 20px 0;
}

.konfirmasi {
    color: #e91e63;
    margin-bottom: 15px;
}

.tombol-konfirmasi button {
    margin: 5px;
    padding: 15px 20px;
    border: none;
    border-radius: 10px;
    background-color: #e91e63;
    color: white;
    cursor: pointer;
}

/* --- Media Queries untuk Responsivitas --- */
@media only screen and (min-width: 324px) {
    .gambar {
        /* Naikkan lebih tinggi di layar kecil */
        bottom: 20%; 
        right: -5%;
        width: 45%; 
        max-width: 17em;
    }

    .judul-kartu, .judul-video {
        font-size: 1.5em;
    }

    .ucapan-teks {
        font-size: 1em; 
    }
}

@media only screen and (max-width: 360px) {
    .gambar {
        /* Naikkan lebih tinggi di layar sangat kecil */
        bottom: 25%; 
        right: -5%;
        width: 45%; 
        max-width: 17em;
    }

    .judul-kartu, .judul-video {
        font-size: 1.5em;
    }

    .ucapan-teks {
        font-size: 1em;
    }
}

@media only screen and (min-width: 480px) {
    .gambar {
        /* Naikkan lebih tinggi di layar besar */
        bottom: 15%; 
        right: -10%;
        width: 40%; 
        max-width: 17em;
    }

    .judul-kartu, .judul-video {
        font-size: 2em; 
    }

    .ucapan-teks {
        font-size: 1.2em;
    }
}

.judul-video {
    font-size: 2em;
}

.konfirmasi {
    font-size: 1.3em;
}
</style>