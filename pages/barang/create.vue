<template>
  <div class="container mx-auto xl:px-20 my-8">
    <h1 class="text-4xl font-bold text-center">Tambah</h1>
    <div class="mt-8 w-full max-w-sm p-6 bg-white border border-gray-200 rounded-lg shadow-lg mx-auto">
      <div v-if="isLoading" class="text-center">
        <Spinner />
      </div>
      <div v-else>
        <form class="space-y-6" action="" method="POST" autocomplete="off" enctype="multipart/form-data" v-on:submit.prevent="tambahBarang">
          <div>
            <label for="nama" class="block mb-2 text-sm font-medium text-gray-900">Nama</label>
            <input type="text" id="nama" v-model="barang.nama" 
                class="bg-gray-50 border-0 text-gray-900 text-sm rounded-lg ring-1 ring-gray-400 focus:ring-2 focus:ring-slate-500 shadow block w-full p-2.5 outline-none">
            <span class="text-red-500" v-if="errors.nama">{{ errors.nama[0] }}</span>
          </div>
          <div>
            <label for="harga" class="block mb-2 text-sm font-medium text-gray-900">Harga</label>
            <input type="number" id="harga" v-model="barang.harga" 
                class="bg-gray-50 border-0 text-gray-900 text-sm rounded-lg ring-1 ring-gray-400 focus:ring-2 focus:ring-slate-500 shadow block w-full p-2.5 outline-none">
            <span class="text-red-500" v-if="errors.harga">{{ errors.harga[0] }}</span>
          </div>
          <div>
            <label for="stok" class="block mb-2 text-sm font-medium text-gray-900">Stok</label>
            <input type="number" id="stok" min="1" v-model="barang.stok"
                class="bg-gray-50 border-0 text-gray-900 text-sm rounded-lg ring-1 ring-gray-400 focus:ring-2 focus:ring-slate-500 shadow block w-full p-2.5 outline-none">
            <span class="text-red-500" v-if="errors.stok">{{ errors.stok[0] }}</span>
          </div>
          <div>
            <label for="stok" class="block mb-2 text-sm font-medium text-gray-900">Kode</label>
            <input type="number" id="stok" min="1" v-model="barang.stok"
                class="bg-gray-50 border-0 text-gray-900 text-sm rounded-lg ring-1 ring-gray-400 focus:ring-2 focus:ring-slate-500 shadow block w-full p-2.5 outline-none">
            <span class="text-red-500" v-if="errors.stok">{{ errors.stok[0] }}</span>
          </div>
          <div>
          </div>
          <div class="flex justify-between">
            <NuxtLink to="/barang"
                class="bg-slate-800 px-5 py-2.5 rounded-lg text-white hover:bg-slate-700 transition duration-150">Kembali</NuxtLink>
            <button type="submit"
                class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 rounded-lg px-5 py-2.5">Tambah</button>
          </div>
        </form>
      </div>
    </div>    
  </div>
</template>

<script setup>
useHead({
  title: 'Tambah Data Barang'
})

const config = useRuntimeConfig()
const { $swal } = useNuxtApp()
const barang = ref({
  barcode: '',
  nama: '',
  harga: '',
  stok: '',
  jenis: '',
  gambar: ''
})
const isLoading = ref(false)
const errors = ref({})

function handleFile(event) {
  barang.value.gambar = event.target.files[0]
}

async function tambahBarang() {
  isLoading.value = true
  try {
    let formData = new FormData()
    formData.append('barcode', barang.value.barcode)
    formData.append('nama', barang.value.nama)
    formData.append('harga', barang.value.harga)
    formData.append('stok', barang.value.stok)
    formData.append('jenis', barang.value.jenis)
    formData.append('gambar', barang.value.gambar)

    const response = await $fetch(`${config.public.apiBase}/api/barang`, {
      method: 'POST',
      body: formData,
      headers: {
        'Accept': 'application/json',
      }
    })
    console.log(response)
    barang.value.barcode = ''
    barang.value.nama = ''
    barang.value.harga = ''
    barang.value.stok = ''
    barang.value.jenis = ''
    barang.value.gambar = ''
    $swal.fire({
      icon: 'success',
      title: 'Sukses',
      text: 'Data barang berhasil ditambahkan.',
    })
  } catch (error) {
    console.log(error)
    console.log(error.data)
    if (error.data.status === 400) {
      errors.value = error.data.errors
    }
  }
  isLoading.value = false
}
</script>