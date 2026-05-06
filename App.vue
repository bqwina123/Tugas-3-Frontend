<script setup>
import { ref, computed } from 'vue'

// --- STATE NAVIGASI ---
const halamanAktif = ref('beranda') // 'beranda', 'katalog', 'daftar'

// --- LOGIKA DATA (REAKTIVITAS) ---
const totalBuku = ref(1247)
const peminjamanAktif = ref(89)
const jumlahAnggota = ref(328)

// Data Dummy Katalog
const koleksiBuku = ref([
  { id: 1, judul: 'Laskar Pelangi', penulis: 'Andrea Hirata', kategori: 'Fiksi' },
  { id: 2, judul: 'Filosofi Teras', penulis: 'Henry Manampiring', kategori: 'Self Dev' },
  { id: 3, judul: 'Bumi', penulis: 'Tere Liye', kategori: 'Fantasi' },
  { id: 4, judul: 'Atomic Habits', penulis: 'James Clear', kategori: 'Edukasi' }
])

// Form Pendaftaran
const formNama = ref('')
const formEmail = ref('')

// Menghitung otomatis buku yang tersedia
const bukuTersedia = computed(() => {
  const hasil = totalBuku.value - peminjamanAktif.value
  return hasil < 0 ? 0 : hasil
})

// Fungsi Pindah Halaman
const navigasiKe = (halaman) => {
  halamanAktif.value = halaman
}

// Fungsi Buat Akun Langsung
const buatAkun = () => {
  if (formNama.value === '' || formEmail.value === '') {
    alert("Mohon isi nama dan email!")
    return
  }
  jumlahAnggota.value++
  alert(`Selamat ${formNama.value}, akun Anda berhasil dibuat!`)
  formNama.value = ''
  formEmail.value = ''
  halamanAktif.value = 'beranda'
}

// Fungsi update angka buku
const updateBuku = (jumlah) => {
  if (totalBuku.value + jumlah >= peminjamanAktif.value) {
    totalBuku.value += jumlah
  } else {
    alert("Buku tersedia tidak boleh minus!")
  }
}
</script>

<template>
  <div class="min-h-screen bg-[#f3f4f6] font-sans pb-20 text-gray-900">
    
    <header class="bg-white px-6 py-12 text-center shadow-sm border-b border-gray-100">
      <div class="flex justify-center mb-4 cursor-pointer" @click="navigasiKe('beranda')">
        <div class="w-16 h-16 bg-red-50 rounded-2xl flex items-center justify-center shadow-inner hover:scale-105 transition-transform">
          <svg class="w-10 h-10 text-red-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" />
          </svg>
        </div>
      </div>
      <h1 class="text-4xl font-extrabold tracking-tight">SiPerpus</h1>
      <p class="text-gray-500 mt-1 font-medium italic">Sistem Informasi Perpustakaan Digital</p>
      
      <div class="mt-8 flex justify-center gap-3">
        <button 
          @click="navigasiKe('katalog')" 
          :class="halamanAktif === 'katalog' ? 'bg-blue-800' : 'bg-blue-600'"
          class="px-6 py-2.5 text-white rounded-lg font-bold shadow-lg shadow-blue-200 hover:bg-blue-700 active:scale-95 transition"
        >
          Jelajahi Katalog
        </button>
        <button 
          @click="navigasiKe('daftar')" 
          :class="halamanAktif === 'daftar' ? 'bg-gray-200 text-gray-800' : 'bg-white text-gray-600'"
          class="px-6 py-2.5 border-2 border-gray-200 rounded-lg font-bold hover:bg-gray-50 active:scale-95 transition"
        >
          Buat Akun
        </button>
      </div>
    </header>

    <main class="max-w-2xl mx-auto px-6 mt-10">
      
      <h2 class="text-xl font-bold text-gray-700 mb-6 flex items-center gap-2">
        <span class="w-1.5 h-6 bg-blue-600 rounded-full"></span>
        Statistik Koleksi
      </h2>

      <div class="space-y-4 mb-10">
        <div class="bg-white p-6 rounded-2xl shadow-sm border border-gray-100 flex items-center gap-6">
          <div class="w-14 h-14 bg-blue-50 text-blue-600 rounded-xl flex items-center justify-center">
            <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" /></svg>
          </div>
          <div>
            <h3 class="text-3xl font-black leading-none">{{ totalBuku }}</h3>
            <p class="text-gray-400 text-sm font-semibold mt-1">Total Judul Buku</p>
          </div>
        </div>

        <div class="bg-white p-6 rounded-2xl shadow-sm border border-gray-100 flex items-center gap-6">
          <div class="w-14 h-14 bg-emerald-50 text-emerald-600 rounded-xl flex items-center justify-center">
            <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M12 4.354a4 4 0 110 5.292M15 21H3v-1a6 6 0 0112 0v1zm0 0h6v-1a6 6 0 00-9-5.197M13 7a4 4 0 11-8 0 4 4 0 018 0z" /></svg>
          </div>
          <div>
            <h3 class="text-3xl font-black leading-none">{{ jumlahAnggota }}</h3>
            <p class="text-gray-400 text-sm font-semibold mt-1">Anggota Terdaftar</p>
          </div>
        </div>

        <div class="bg-white p-6 rounded-2xl shadow-sm border border-gray-100 flex items-center gap-6">
          <div class="w-14 h-14 bg-orange-50 text-orange-600 rounded-xl flex items-center justify-center">
            <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" /></svg>
          </div>
          <div>
            <h3 class="text-3xl font-black leading-none">{{ peminjamanAktif }}</h3>
            <p class="text-gray-400 text-sm font-semibold mt-1">Buku Sedang Dipinjam</p>
          </div>
        </div>

        <div class="bg-indigo-600 p-6 rounded-2xl shadow-lg shadow-indigo-100 flex items-center justify-between group">
          <div class="flex items-center gap-6 text-white">
            <div class="w-14 h-14 bg-white/20 rounded-xl flex items-center justify-center">
              <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>
            </div>
            <div>
              <h3 class="text-3xl font-black leading-none">{{ bukuTersedia }}</h3>
              <p class="text-indigo-100 text-sm font-semibold mt-1">Buku Siap Pinjam</p>
            </div>
          </div>
          <span class="text-[10px] bg-white/20 text-white px-2 py-1 rounded font-bold uppercase tracking-tighter">Live System</span>
        </div>
      </div>

      <transition name="fade" mode="out-in">
        
        <section v-if="halamanAktif === 'katalog'" key="katalog" class="animate-in">
          <h3 class="text-xl font-bold text-gray-800 mb-4 text-center md:text-left">📚 Katalog Koleksi</h3>
          <div class="grid grid-cols-1 gap-3">
            <div v-for="buku in koleksiBuku" :key="buku.id" class="bg-white p-4 rounded-xl border border-gray-200 flex justify-between items-center hover:shadow-md transition-shadow">
              <div>
                <p class="font-bold text-gray-800">{{ buku.judul }}</p>
                <p class="text-xs text-gray-500">{{ buku.penulis }} | <span class="text-blue-600 font-semibold">{{ buku.kategori }}</span></p>
              </div>
              <button class="text-xs bg-gray-100 px-3 py-1.5 rounded-md font-bold text-gray-600 hover:bg-blue-600 hover:text-white transition">Pinjam</button>
            </div>
          </div>
        </section>

        <section v-else-if="halamanAktif === 'daftar'" key="daftar" class="bg-white p-8 rounded-3xl shadow-sm border border-gray-100 animate-in">
          <div class="text-center mb-6">
            <h3 class="font-bold text-gray-800 text-xl">Daftar Anggota Baru</h3>
            <p class="text-sm text-gray-400">Silahkan isi data diri Anda untuk bergabung</p>
          </div>
          <div class="space-y-4 max-w-sm mx-auto">
            <input v-model="formNama" type="text" placeholder="Nama Lengkap" class="w-full px-4 py-3 bg-gray-50 border border-gray-200 rounded-xl outline-none focus:ring-2 focus:ring-blue-500 transition">
            <input v-model="formEmail" type="email" placeholder="Alamat Email" class="w-full px-4 py-3 bg-gray-50 border border-gray-200 rounded-xl outline-none focus:ring-2 focus:ring-blue-500 transition">
            <button @click="buatAkun" class="w-full py-4 bg-blue-600 text-white rounded-xl font-bold shadow-lg hover:bg-blue-700 transition transform active:scale-95">Konfirmasi Pendaftaran</button>
          </div>
        </section>

        <section v-else key="beranda" class="mt-4 bg-white p-8 rounded-3xl border border-dashed border-gray-300 animate-in">
          <div class="text-center mb-6">
            <h3 class="font-bold text-gray-800">Panel Kontrol Data</h3>
            <p class="text-xs text-gray-400 uppercase tracking-widest mt-1">Ubah data di bawah untuk melihat reaktivitas</p>
          </div>

          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div class="p-4 bg-gray-50 rounded-2xl">
              <label class="block text-xs font-black text-gray-400 uppercase mb-3 text-center">Total Koleksi Buku</label>
              <div class="flex items-center justify-center gap-4">
                <button @click="updateBuku(-1)" class="w-10 h-10 bg-white border border-gray-200 rounded-lg hover:bg-red-50 hover:text-red-600 transition shadow-sm font-bold">-</button>
                <input type="number" v-model.number="totalBuku" class="w-20 text-center font-black text-xl text-gray-700 bg-transparent focus:outline-none">
                <button @click="updateBuku(1)" class="w-10 h-10 bg-white border border-gray-200 rounded-lg hover:bg-green-50 hover:text-green-600 transition shadow-sm font-bold">+</button>
              </div>
            </div>

            <div class="p-4 bg-gray-50 rounded-2xl">
              <label class="block text-xs font-black text-gray-400 uppercase mb-3 text-center">Simulasi Buku Terpinjam</label>
              <input type="range" min="0" :max="totalBuku" v-model.number="peminjamanAktif" class="w-full h-2 bg-blue-100 rounded-lg appearance-none cursor-pointer accent-blue-600">
              <p class="text-center text-xs font-bold text-blue-600 mt-2">{{ peminjamanAktif }} Buku Terpinjam</p>
            </div>
          </div>
        </section>
      </transition>
    </main>
  </div>
</template>

<style scoped>
/* Menghilangkan panah pada input number */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Animasi sederhana */
.animate-in {
  animation: fadeIn 0.4s ease-out;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

/* Vue Transition */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>
