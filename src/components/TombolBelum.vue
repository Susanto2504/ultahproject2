<script setup>
import { defineEmits } from 'vue';
const emit = defineEmits(['kembali']);
</script>

<template>
  <div class="halaman-nakal-container">
    <div class="pesan-nakal">
      <h2 class="teks-nakal">Kamu Nakal Yaa!!!</h2>
      <h2 class="teks-nakal">Kembali Nonton Dulu Sana</h2>
      <button class="btn-kembali" @click="emit('kembali')">Kembali</button>
    </div>
  </div>
</template>

<style scoped>
/* Tambahkan CSS untuk Halaman 5 (Kamu Nakal) di sini */
.halaman-nakal-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    width: 100vw; 
    text-align: center;
}
.pesan-nakal {
    color: #e91e63; 
    font-weight: bold;
}
.teks-nakal {
    font-size: 2em;
    margin: 5px 0;
}
.btn-kembali {
    padding: 10px 20px;
    background-color: #e91e63;
    color: white;
    border: none;
    border-radius: 15px;
    cursor: pointer;
    font-size: 1em;
    margin-top: 30px;
}
</style>