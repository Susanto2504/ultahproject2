<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
    kadoAktif: Number
});

const emit = defineEmits(['buka-kado']);

function bukaKado() {
    emit('buka-kado');
}
</script>

<template>
    <div class="halaman-siapkan-container">
        <div class="content-box">
            <p>Siapkan Kado Nomor {{ kadoAktif }} Yaa!</p>
            <button class="btn-buka-kado" @click="bukaKado">
                Buka Kado
            </button>
        </div>
    </div>
</template>

<style scoped>
.halaman-siapkan-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    padding: 20px;
}

.content-box {
    background-color: white;
    border-radius: 15px;
    padding: 40px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    text-align: center;
    width: 80%;
    max-width: 350px;
}

p {
    font-size: 1.5em;
    font-weight: bold;
    color: #e91e63;
    margin-bottom: 30px;
}

.btn-buka-kado {
    padding: 15px 40px;
    background-color: #e91e63; /* Warna cokelat marun */
    color: white;
    border: none;
    border-radius: 30px;
    cursor: pointer;
    font-size: 1.1em;
    font-weight: bold;
    transition: background-color 0.3s;
}

.btn-buka-kado:hover {
    background-color: #7b2938;
}
</style>