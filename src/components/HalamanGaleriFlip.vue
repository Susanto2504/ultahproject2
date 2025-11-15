<script setup>
import { ref, defineProps, defineEmits } from 'vue';

const emit = defineEmits(['lanjut']);

const props = defineProps({
    fotoData: {
        type: Array,
        required: true
    },
    judul: String
});

const isFlipped = ref({}); 

function flipCard(id) {
    if (isFlipped.value[id] === undefined) {
        isFlipped.value[id] = false;
    }
    
    // 2. KONDISI PENTING: HANYA izinkan flip jika kartu BELUM PERNAH ter-flip (false)
    if (!isFlipped.value[id]) {
        isFlipped.value[id] = true; // Set menjadi true (ter-flip)
    } 
}
</script>

<template>
  <div class="halaman-galeri-container">
    <h1 class="judul-galeri">FOTO TERCANTIK KAMU!</h1>
    
    <div class="galeri-grid">
      
      <div v-for="item in fotoData" :key="item.id" class="kotak-luar" @click="flipCard(item.id)">
        
        <div class="kartu-3d" :class="{ 'flipped': isFlipped[item.id] }">
          
          <div class="sisi sisi-depan">
            <p>{{ item.teks || 'Klik Disini' }}</p>
          </div>
          
          <div class="sisi sisi-belakang">
            <img :src="item.foto" :alt="'Foto ' + item.id" class="foto-kecil">
          </div>
        </div>
      </div>
      
    </div>
    
    <button class="btn-lanjut-galeri" @click="emit('lanjut')">Lanjut</button>
  </div>
</template>

<style scoped>
.halaman-galeri-container {
    justify-content: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 60px;
    min-height: 100vh;
}

.judul-galeri {
    color: #e91e63;
    margin-bottom: 25px;
    font-size: 1.2em;
    text-align: center;
}

.galeri-grid {
    display: grid;
    /* Grid 3 kolom, ukuran kotak disesuaikan */
    grid-template-columns: repeat(2, 1fr); 
    gap: 15px;
    width: 95%;
    max-width: 650px;
    margin: 0; /* Lebar maksimal agar tidak terlalu besar di ponsel */
}

.kotak-luar {
    /* Container luar untuk ukuran kotak */
    width: 100%;
    padding-top: 133.33%; /* Membuat kotak menjadi persegi */
    position: relative;
    cursor: pointer;
    animation: fadeInUp 0.5s ease-out forwards;
    opacity: 0; /* Mulai dari tidak terlihat */
}

@keyframes fadeInUp {
    from {
        opacity: 0;
        transform: translateY(20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}


/* ------------------- EFEK FLIP 3D ------------------- */
.kartu-3d {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    transition: transform 0.6s;
    transform-style: preserve-3d;
}

.kartu-3d.flipped {
    transform: rotateY(180deg);
}

.sisi {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden; /* Menyembunyikan sisi belakang saat tidak berputar */
    border-radius: 12px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: bold;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.sisi-depan {
    background-color: #e91e63; /* Warna tombol/kue */
    color: white;
    font-size: 0.8em;
    text-align: center;
}

.sisi-belakang {
    background-color: #f0f0f0;
    transform: rotateY(180deg);
    overflow: hidden;
}

.foto-kecil {
    width: 100%;
    height: 100%;
    object-fit: cover; /* Memastikan foto mengisi kotak */
    border-radius: 12px;
}

/* ------------------- TOMBOL LANJUT ------------------- */
.btn-lanjut-galeri {
    margin-top: 35px;
    padding: 12px 30px;
    background-color: #e91e63;
    color: white;
    border: none;
    border-radius: 25px;
    cursor: pointer;
    font-size: 1.1em;
    transition: background-color 0.3s;
    animation: fadeInUp 0.5s ease-out forwards;
    opacity: 0;
    animation-delay: 1.5s;
}
</style>