<script setup>
import { ref, watch } from 'vue';
import HalamanLogin from './components/HalamanLogin.vue';
import HalamanSpesial from './components/HalamanSpesial.vue'; 
import HalamanKue from './components/HalamanKue.vue';
import HalamanGaleriFlip from './components/HalamanGaleriFlip.vue';
import HalamanVideoFinal from './components/HalamanVideoFinal.vue';
import HalamanLihatKado from './components/HalamanLihatKado.vue'; // Halaman 10
import HalamanSiapkanKado from './components/HalamanSiapkanKado.vue'; // Halaman 11
import HalamanIsiKado from './components/HalamanIsiKado.vue'; // Halaman 12
import HalamanSelesaiKado from './components/HalamanSelesaiKado.vue'; // Halaman 13
import HalamanVideoPenutup from './components/HalamanVideoPenutup.vue';

const statusLogin = ref(false); 
const halamanAktif = ref(1); 
const fotoHalaman7 = ref([
    { id: 701, foto: '/Poster Ucapan Memperingati Maulid Nabi Muhammad SAW Illustratif Imut Warna-Warni.png', teks: 'Klik Disini' },
    { id: 702, foto: '/1.jpg', teks: 'Klik Disini' },
    { id: 703, foto: '/2.jpg', teks: 'Klik Disini' },
    { id: 704, foto: '/3.jpg', teks: 'Klik Disini' },
    { id: 705, foto: '/4.jpg', teks: 'Klik Disini' },
    { id: 706, foto: '/5.jpg', teks: 'Klik Disini' },
]);

const fotoHalaman8 = ref([
    { id: 801, foto: '/6.jpg', teks: 'Klik Disini' },
    { id: 802, foto: '/7.jpg', teks: 'Klik Disini' },
    { id: 803, foto: '/8.jpg', teks: 'Klik Disini' },
    { id: 804, foto: '/9.jpg', teks: 'Klik Disini' },
    { id: 805, foto: '/10.jpg', teks: 'Klik Disini' },
    { id: 806, foto: '/11.jpg', teks: 'Klik Disini' },
]);

const KUNCI_KADO = {
    1: 'GANTUNGAN KUNCI',       // Kunci untuk Kado 1
    2: 'CERMIN',        // Kunci untuk Kado 2
    3: 'BAJU',      // Kunci untuk Kado 3
    4: 'SET BAJU',       // Kunci untuk Kado 4
    5: 'SEPATU'      // Kunci untuk Kado 5
};

const VIDEO_KADO = {
    1: '/Kado 1.mp4',       // Video untuk Kado 1
    2: '/Kado 2.mp4',       // Video untuk Kado 2
    3: '/Kado 3.mp4',       // Video untuk Kado 3
    4: '/Kado 4.mp4',       // Video untuk Kado 4
    5: '/Kado 5.mp4'        // Video untuk Kado 5
};

const PESAN_CINTA_KADO = {
    1: 'Pake ya sayang gantungan kuncinya, kubeli ini karena mirip kamu imut, cantik, dan lucuu🤭🤭',
    2: 'Kado kedua ini biar kamu punya cermin baru ditambah ada foto kamu loh yang paling aku suka 🥰🥰',
    3: 'Ini dia baju yang aku belikan buat kamu, ulang tahun kemarin kamu bilang "Boneka terus, kapan baju", nihh sekarang ku kasih baju walau baju hitammu dah banyak sih😭😭',
    4: 'Kado keempat ini menurutku agak spesial sih karena isinya satu set baju, semoga aja kamu suka yaa😆😆',
    5: 'Kado terakhir ini agak bigung sebenarnya jadi ya aku kasih aja sepatu putih, abisnya waktu kamu datang tuh kamu ngeluh sih sepatu kamu dah kotor, semoga kamu suka ya sepatunya😓😓'
};

const kadoAktif = ref(1);

// --- Konstanta Audio ---
const LAGU_ULANG_TAHUN = '/Lilinnya.mp3'; 
const LAGU_CINTA_1 = '/Alamak.dat.mp3'; 
const LAGU_CINTA_2 = '/Audio WhatsApp 2025-10-14 pukul 03.59.26_6c773a18.dat.mp3';

const audioUltah = ref(null); // Objek Audio untuk lagu Happy Birthday
let isAudioUltahReady = false; // Flag untuk memastikan audio sudah dimuat

let audioLagu1 = null; 
let audioLagu2 = null;

const audioCinta = ref(null); 
const laguDihentikanSementara = ref(false);

// --- Inisialisasi dan Logika Bersambung ---
function inisialisasiDanPutarAudioCinta() {
    // Cek apakah inisialisasi sudah dilakukan
    if (audioLagu1) {
        // Jika sudah diinisialisasi, langsung mainkan lagu yang sedang aktif
    audioCinta.value.play().catch(e => console.error("Gagal putar lagu cinta:", e));
    return;
    }

    // 1. Inisialisasi Objek Audio Pertama Kali
    audioLagu1 = new Audio(LAGU_CINTA_2); // Lagu 1 (Starter)
    audioLagu2 = new Audio(LAGU_CINTA_1); // Lagu 2 (Lanjutan)

    // Pastikan loop dimatikan agar event 'ended' terpicu
    audioLagu1.loop = false; 
    audioLagu2.loop = false; 

    // 2. Logika Chain Loop: Lagu 1 -> Lagu 2
    audioLagu1.addEventListener('ended', () => {
    console.log('Lagu Cinta 1 selesai, memutar Lagu Cinta 2');
    audioCinta.value = audioLagu2; // Alihkan pointer ke Lagu 2
    audioLagu2.currentTime = 0; 
    audioLagu2.play().catch(e => console.error("Gagal putar lagu 2:", e));
    });
    
    // 3. Logika Chain Loop: Lagu 2 -> Lagu 1 (KUNCI LOOP TAK TERBATAS)
    audioLagu2.addEventListener('ended', () => {
        console.log('Lagu Cinta 2 selesai, kembali memutar Lagu Cinta 1');
        audioCinta.value = audioLagu1; // Alihkan pointer kembali ke Lagu 1
        audioLagu1.currentTime = 0; 
        audioLagu1.play().catch(e => console.error("Gagal putar lagu 1:", e));
 });

    // 4. Set pointer awal ke Lagu 1 dan putar
    audioCinta.value = audioLagu1;

    // Mulai putar (Catatan: Ini akan diblokir browser sampai ada klik)
    audioCinta.value.play().catch(e => {
    console.log("Audio siap, tunggu interaksi klik di HalamanSpesial.");
 });
}

audioUltah.value = new Audio(LAGU_ULANG_TAHUN);
audioUltah.value.loop = true; // Loop agar lagu berputar terus
audioUltah.value.volume = 0.6; 
audioUltah.value.addEventListener('canplaythrough', () => {
    isAudioUltahReady = true;
    console.log('Lagu Ulang Tahun siap diputar.');
});



// --- Fungsi Navigasi dari Halaman Login ---
function masukDashboard() {
    statusLogin.value = true;
    halamanAktif.value = 2; 

    inisialisasiDanPutarAudioCinta();
}

function nextHalaman() {
    if (halamanAktif.value === 2) {
        halamanAktif.value = 3; 
    }
}
function kembaliDulu() {
    if (halamanAktif.value === 3) {
        halamanAktif.value = 4; // Video -> Masukkan Kode
    }
}

// 3. BARU: Dari Halaman 3 (tombol 'Sudah') -> Halaman 5
function selesaiVideo() {
    if (halamanAktif.value === 3) {
        halamanAktif.value = 5; 
    }
}

function kembaliKeVideo() {
    if (halamanAktif.value === 4) {
        halamanAktif.value = 3; 
    }
}

// 5. BARU: Dari Halaman 4 (tombol 'Lanjut') -> Halaman Baru (misalnya Halaman 6)
function lanjutDariKode() {
    if (halamanAktif.value === 5) {
        halamanAktif.value = 6; 
        console.log("Kode dimasukkan, pindah ke Halaman 6");
    }
}

function kembaliDariKode() {
    if (halamanAktif.value === 5) {
        halamanAktif.value = 3; // Pindah kembali ke Halaman 3 (Video)
    }
}

watch(halamanAktif, (newValue, oldValue) => {
    
    // KONDISI 1: MASUK KE HALAMAN 6 (Ganti Lagu)
    if (newValue === 6) {
        // Hentikan Lagu Cinta (jika sedang main)
        if (audioCinta.value && !audioCinta.value.paused) {
            audioCinta.value.pause();
            laguDihentikanSementara.value = true;
            console.log('Lagu Cinta dihentikan.');
        }

        // Putar Lagu Ulang Tahun
        if (audioUltah.value && isAudioUltahReady) {
            audioUltah.value.play().catch(e => console.error("Gagal putar Lagu Ulang Tahun:", e));
            console.log('Lagu Ulang Tahun dimulai.');
        } else {
             // Coba putar lagi jika belum siap (fallback)
            audioUltah.value.play().catch(e => console.error("Gagal putar Lagu Ulang Tahun (fallback):", e));
        }
    }

    if (oldValue === 6 && newValue === 7) {
    // Hentikan Lagu Ulang Tahun
    if (audioUltah.value) {
        audioUltah.value.pause();
        audioUltah.value.currentTime = 0; // Tetap reset lagu ultah
        console.log('Lagu Ulang Tahun dihentikan.');
    }
    
    // AKTIFKAN: Lanjutkan Lagu Cinta dari currentTime yang tersimpan
    if (laguDihentikanSementara.value && audioCinta.value) {
        audioCinta.value.play().catch(e => console.error("Gagal melanjutkan Lagu Cinta setelah Halaman 6:", e));
        laguDihentikanSementara.value = false;
        console.log('Lagu Cinta dilanjutkan dari posisi terakhir di Halaman 7.');
    }
}
});

function lanjutKeFinal() {
    if (audioUltah.value) {
            audioUltah.value.pause();
            audioUltah.value.currentTime = 0;
            console.log("Lagu Ulang Tahun dihentikan saat pindah ke Halaman 7.");
        }
        
        // Lanjutkan navigasi
        halamanAktif.value = 7; // Pindah ke Halaman 7
        console.log("Pindah ke Halaman 7!");
}

function lanjutDariGaleri() {
    if (halamanAktif.value === 7) {
        halamanAktif.value = 8; // Dari Halaman 7 ke Halaman 8
        console.log("Pindah ke Halaman 8 (Galeri 2)");
    } else if (halamanAktif.value === 8) {
        halamanAktif.value = 9; // Dari Halaman 8 ke Halaman 9 (Final)
        console.log("Pindah ke Halaman 9 (Final)");
    }
}

// Fungsi navigasi untuk Galeri -> Video Final
function lanjutDariGaleri2() {
    if (halamanAktif.value === 7) {
        halamanAktif.value = 8; 
    } else if (halamanAktif.value === 8) {
        halamanAktif.value = 9; // Pindah dari Galeri 2 ke Halaman 9 (Video Final)
        console.log("Pindah ke Halaman 9 (Video Final)");
    }
}

// NEW FUNCTION: Navigasi dari Video Final ke Halaman 10 (Final Page)
function lanjutDariVideoFinal() {
    if (halamanAktif.value === 9) {
        halamanAktif.value = 10; // Pindah ke Halaman 10 (Halaman Final)
        console.log("Pindah ke Halaman 10 (Final Page)");
    }
}

function pilihKado(nomorKado) {
    // Pastikan user hanya bisa memilih kado yang aktif
    if (nomorKado === kadoAktif.value) {
        halamanAktif.value = 11; // Pindah ke Halaman 11 (Siapkan Kado)
        console.log(`Memilih Kado ${nomorKado}`);
    } else {
        console.log(`Kado ${nomorKado} belum tersedia.`);
    }
}

function bukaKado() {
    halamanAktif.value = 12; // Pindah ke Halaman 12 (Isi Kado)
    console.log(`Membuka Isi Kado ${kadoAktif.value}`);
}

// Dipanggil dari Halaman 12 (Isi Kado, tombol "Lihat Pesan" / "Lanjut")
function lihatPesanKado(jawabanUser) { // TERIMA JAWABAN DARI KOMPONEN ANAK
    const kunciYangBenar = KUNCI_KADO[kadoAktif.value];
    
    // Validasi Jawaban
    if (jawabanUser.toUpperCase().trim() === kunciYangBenar) {
        // Jawaban Benar
        halamanAktif.value = 13; // Pindah ke Halaman 13 (Selesai Kado)
        console.log(`Jawaban Kado ${kadoAktif.value} BENAR! Pindah ke Halaman 13.`);
    } 
}


function selesaiKado() {
    
    // 1. Cek apakah ada kado selanjutnya (Total 5 Kado)
    if (kadoAktif.value < 5) { 
        
        // 2. Maju ke kado berikutnya
        kadoAktif.value++; 
        
        // 3. Kembali ke Halaman 10 (Tampilan list kado)
        halamanAktif.value = 10;
        console.log(`Selesai Kado, Lanjut ke Kado ${kadoAktif.value}`);
        
    } else {
        // SEMUA KADO SELESAI
        console.log("Semua Kado Selesai! Pindah ke Halaman Video Penutup.");
        halamanAktif.value = 14; // << BARU: Pindah ke Halaman 14
        
        // Opsional: Hentikan Lagu Cinta jika sudah selesai
        
    }
}

</script>

<template>
  <main>
    <HalamanLogin 
      v-if="halamanAktif === 1" 
      @login-sukses="masukDashboard"
    />

    <HalamanSpesial
    v-else-if="halamanAktif >= 2 && halamanAktif <= 5"
    :halaman="halamanAktif"
    :audioCinta="audioCinta"
    @next="nextHalaman"
    @prev="kembaliDulu"      
    @selesai="selesaiVideo" 
    @kembali="kembaliKeVideo" 
    @lanjut="lanjutDariKode"
    @kembaliVideo="kembaliDariKode"  
    />

    <HalamanKue
        v-else-if="halamanAktif === 6"
        @lanjut="lanjutKeFinal"
    />

    <HalamanGaleriFlip
        v-else-if="halamanAktif === 7"
        :fotoData="fotoHalaman7"
        judul="Momen Indah Kita (Part 1)"
        @lanjut="lanjutDariGaleri"
    />

    <HalamanGaleriFlip
        v-else-if="halamanAktif === 8"
        :fotoData="fotoHalaman8"
        judul="Momen Indah Kita (Part 2)"
        @lanjut="lanjutDariGaleri"
    />

    <HalamanVideoFinal
        v-else-if="halamanAktif === 9"
        :audioCinta="audioCinta"
        @lanjut="lanjutDariVideoFinal"
    />

    <HalamanLihatKado
        v-else-if="halamanAktif === 10"
        :kadoAktif="kadoAktif"
        @pilih-kado="pilihKado"
    />

    <HalamanSiapkanKado
        v-else-if="halamanAktif === 11"
        :kadoAktif="kadoAktif"
        @buka-kado="bukaKado"
    />

    <HalamanIsiKado
        v-else-if="halamanAktif === 12"
        :kadoAktif="kadoAktif"
        :kunciJawaban="KUNCI_KADO[kadoAktif]"
        @lanjut="lihatPesanKado"
    />

    <HalamanSelesaiKado
        v-else-if="halamanAktif === 13"
        :kadoAktif="kadoAktif"
        @selesai="selesaiKado"
        :audioCinta="audioCinta"
        :pesanCinta="PESAN_CINTA_KADO[kadoAktif]"
        :videoSource="VIDEO_KADO[kadoAktif]"
    />

    <HalamanVideoPenutup
        v-else-if="halamanAktif === 14"
        :audioCinta="audioCinta"
    />
</main>
</template>