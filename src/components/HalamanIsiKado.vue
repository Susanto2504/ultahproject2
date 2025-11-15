<script setup>
import { defineProps, defineEmits, ref } from 'vue';

const props = defineProps({
    kadoAktif: Number,
    kunciJawaban: String
});

const emit = defineEmits(['lanjut']);

const jawabanUser = ref('');
const pesanError = ref('');

function lihatPesan() {
    
    // Reset pesan error
    pesanError.value = '';

    const jawabanDibersihkan = jawabanUser.value.toUpperCase().trim();
    
    // Pilihan A: Validasi di sini, lalu emit jika benar (Lebih baik, memisahkan logika)
    if (jawabanDibersihkan === props.kunciJawaban.toUpperCase()) {
        
        // Jawaban BENAR, kirim jawaban ke App.vue untuk transisi
        emit('lanjut', jawabanUser.value);
        
    } else if (!jawabanDibersihkan) {
        pesanError.value = 'Jawaban tidak boleh kosong!';
        return;
    } else {
        // Jawaban SALAH, tampilkan pesan error
        pesanError.value = 'Jawaban salah! Coba lagi.';
    }

    if (jawabanDibersihkan) {
        emit('lanjut', jawabanUser.value);
    } else {
        pesanError.value = 'Jawaban tidak boleh kosong!';
    }
}
</script>

<template>
    <div class="halaman-isi-kado-container">
        <h1 class="judul">Apa Tuh Isi Kadonya?</h1>
        
        <input 
            type="text" 
            placeholder="Ketik Jawaban/Kode" 
            v-model="jawabanUser"
            class="input-kado" 
            @keyup.enter="lihatPesan"
        />

        <p v-if="pesanError" class="pesan-error">{{ pesanError }}</p>
        
        <button class="btn-lihat-pesan" @click="lihatPesan">
            Lihat Pesan
        </button>
    </div>
</template>

<style scoped>
.halaman-isi-kado-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    padding: 20px;
}

.judul {
    font-size: 1.5em;
    font-weight: bold;
    color: #e91e63;
    margin-bottom: 30px;
}

.input-kado {
    width: 80%;
    max-width: 300px;
    padding: 15px;
    border: none;
    border-radius: 10px;
    font-size: 1em;
    text-align: center;
    margin-bottom: 10px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.btn-lihat-pesan {
    padding: 15px 40px;
    background-color: #e91e63;
    color: white;
    border: none;
    border-radius: 30px;
    cursor: pointer;
    font-size: 1.1em;
    font-weight: bold;
    transition: background-color 0.3s;
}

.btn-lihat-pesan:hover {
    background-color: #7b2938;
}

.pesan-error {
    color: #e91e63; /* Merah untuk pesan error */
    font-weight: bold;
    margin-bottom: 30px;
}
</style>