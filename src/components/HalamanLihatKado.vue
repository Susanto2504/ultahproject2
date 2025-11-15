<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
    kadoAktif: {
        type: Number,
        required: true
    }
});

const emit = defineEmits(['pilih-kado']);

// Data daftar kado
const daftarKado = [1, 2, 3, 4, 5];

function handlePilihKado(nomor) {
    // Hanya izinkan klik pada kado yang aktif
    if (nomor === props.kadoAktif) {
        emit('pilih-kado', nomor);
    }
}
</script>

<template>
    <div class="halaman-kado-container">
        <h1 class="judul-kado">LIHAT KADO</h1>
        
        <div class="daftar-kado">
            <div 
                v-for="nomor in daftarKado" 
                :key="nomor" 
                class="item-kado"
                :class="{ 'aktif': nomor === kadoAktif, 'selesai': nomor < kadoAktif }"
                @click="handlePilihKado(nomor)">
                
                <span :class="{ 'kado-aktif': nomor === kadoAktif }">Kado {{ nomor }}</span>
                <span class="icon-panah">&gt;</span>
            </div>
        </div>
    </div>
</template>

<style scoped>
/* Styling didasarkan pada gambar yang Anda berikan (pink background, font tebal) */
.halaman-kado-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    color: #4a2335; /* Dark brown/maroon font */
    padding: 20px;
}

.judul-kado {
    font-size: 1.8em;
    font-weight: bold;
    margin-bottom: 50px;
    font-family: 'Georgia', serif; /* Meniru font tebal/serif */
}

.daftar-kado {
    width: 80%;
    max-width: 300px;
}

.item-kado {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 1.5em;
    font-weight: bold;
    margin: 20px 0;
    padding: 5px 0;
    cursor: default;
    border-bottom: 1px solid rgba(74, 35, 53, 0.3);
    transition: color 0.3s;
}

.item-kado.aktif {
    color: #e91e63; /* Warna aktif/merah */
    cursor: pointer;
}

.item-kado.aktif:hover {
    color: #7b1fa2;
}

.item-kado.selesai {
    color: #888; /* Kado yang sudah selesai sedikit pudar */
}

.icon-panah {
    font-weight: lighter;
}

.kado-aktif {
    /* Ini untuk meniru font tebal yang Anda gunakan */
    font-family: 'Georgia', serif; 
    font-weight: 900;
}
</style>