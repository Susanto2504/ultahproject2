<script setup>
import { ref, defineEmits } from 'vue';
// Anda bisa menambahkan logika kode di sini nanti
const emit = defineEmits(['lanjut', 'kembaliVideo']); 
const KODE_RAHASIA = "AKUSAYANGCHALVIN";
const kodeInput = ref('');
const pesanError = ref('');
function cekKode() {
    // Menghapus spasi di awal/akhir dan mengubah ke huruf besar agar tidak sensitif
    const kodeBersih = kodeInput.value.trim().toUpperCase();
    
    if (kodeBersih === KODE_RAHASIA) {
        // Jika kode benar, panggil event 'lanjut' ke App.vue
        pesanError.value = '';
        emit('lanjut');
    } else {
        // Jika kode salah, tampilkan pesan error
        pesanError.value = 'Hayoloh, katanya dah nonton videonya!';
    }
}
</script>

<template>
  <div class="halaman-kode-container">
    <div class="input-kode-box">
    <p class="instruksi-kode">Masukkan Kode</p>
      <input type="text" class="input-kode" placeholder="Masukkan Kodenya Sayang" v-model="kodeInput"  @keyup.enter="cekKode"> 

        <p v-if="pesanError" class="pesan-error">{{ pesanError }}</p>

            <div class="btn">
                <button class="btn-lanjut" @click="cekKode">Lanjut</button>
              
                <button class="btn-kembali" @click="emit('kembaliVideo')">Kembali</button>
            </div>
  </div>
  </div>
</template>

<style scoped>
.halaman-kode-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100%;
    width: 100%;/* Sesuaikan dengan warna pink */
    text-align: center;
}
.input-kode-box {
    width: 100%;
    max-width: 400px;
    display: flex;
    flex-direction: column;
    align-items: center; 
}
.instruksi-kode {
    color:#e91e63;
    font-size: 1.5em;
    font-weight: bold;
    margin-bottom: 20px;
}

.input-kode {
    width: 70%;
    padding: 15px;
    border: none;
    border-radius: 25px;
    margin-bottom: 10px;
    text-align: center;
    font-size: 1em;
    outline: none;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.input-kode::placeholder {
    font-size: 0.8em;
    color: #888;
}



.btn {
    display: flex;
    justify-content: center;
    gap: 5px;
    margin-top: 10px;
    /* width: content; */
}

.btn-lanjut {
    padding: 12px 30px;
    background-color: #e91e63; 
    color: white;
    border: none;
    border-radius: 25px;
    cursor: pointer;
    font-size: 1.1em;
    margin-top: 20px;
    width: auto;
}

.btn-kembali {
    padding: 12px 30px;
    background-color: #e91e63; 
    color: white;
    border: none;
    border-radius: 25px;
    cursor: pointer;
    font-size: 1.1em;
    margin-top: 20px;
    width: auto;
}

.pesan-error {
    color: #f100009a; /* Merah untuk error */
    font-size: 0.9em;
    font-weight: bold;
    margin-top: -5px; /* Sedikit naik di atas tombol */
    margin-bottom: 10px;
}
</style>