<template>
  <div class="container mx-auto xl:px-20 my-8">
    <h1 class="text-4xl font-bold text-center text-blue-700">Tambah</h1>
    <div class="mt-8 max-w-lg mx-auto bg-gray-100 border border-gray-300 rounded-lg shadow-lg">
      <div v-if="isLoading" class="text-center py-6">
        <Spinner />
      </div>
      <div v-else>
        <form class="p-6 space-y-6" action="" method="POST" autocomplete="off" enctype="multipart/form-data" v-on:submit.prevent="tambahBarang">
          <div>
            <label for="nama" class="block mb-2 text-sm font-medium text-gray-900">Nama Barang</label>
            <input type="text" id="nama" v-model="barang.nama"
                class="bg-white border-2 border-gray-300 rounded-lg focus:outline-none focus:border-blue-500 shadow-inner block w-full px-4 py-2">
            <span class="text-red-500" v-if="errors.nama">{{ errors.nama[0] }}</span>
          </div>
          <div>
            <label for="harga" class="block mb-2 text-sm font-medium text-gray-900">Harga Barang</label>
            <input type="number" id="harga" v-model="barang.harga"
                class="bg-white border-2 border-gray-300 rounded-lg focus:outline-none focus:border-blue-500 shadow-inner block w-full px-4 py-2">
            <span class="text-red-500" v-if="errors.harga">{{ errors.harga[0] }}</span>
          </div>
          <div>
            <label for="stok" class="block mb-2 text-sm font-medium text-gray-900">Stok Barang</label>
            <input type="number" id="stok" min="1" v-model="barang.stok"
                class="bg-white border-2 border-gray-300 rounded-lg focus:outline-none focus:border-blue-500 shadow-inner block w-full px-4 py-2">
            <span class="text-red-500" v-if="errors.stok">{{ errors.stok[0] }}</span>
          </div>
          <div>
            <label for="kode" class="block mb-2 text-sm font-medium text-gray-900">Kode Barang</label>
            <input type="number" id="kode" min="1" v-model="barang.kode"
                class="bg-white border-2 border-gray-300 rounded-lg focus:outline-none focus:border-blue-500 shadow-inner block w-full px-4 py-2">
            <span class="text-red-500" v-if="errors.kode">{{ errors.kode[0] }}</span>
          </div>
          <div class="flex justify-between">
            <NuxtLink to="/barang"
                class="bg-blue-500 hover:bg-blue-600 text-white rounded-lg px-5 py-2.5 transition duration-150">Kembali</NuxtLink>
            <button type="submit"
                class="bg-blue-700 hover:bg-blue-800 text-white rounded-lg px-5 py-2.5 focus:outline-none focus:ring-2 focus:ring-blue-500">Tambah</button>
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
  kode: '',
  nama: '',
  harga: '',
  stok: '',
  jenis: '',
})
const isLoading = ref(false)
const errors = ref({})

async function tambahBarang() {
  isLoading.value = true
  try {
    let formData = new FormData()
    formData.append('code', barang.value.kode)
    formData.append('name', barang.value.nama)
    formData.append('price', barang.value.harga)
    formData.append('stock', barang.value.stok)

    const response = await $fetch(`${config.public.apiBase}/api/product`, {
      method: 'POST',
      body: formData,
      headers: {
        'Accept': 'application/json',
      }
    })
    console.log(response)
    barang.value.kode = ''
    barang.value.nama = ''
    barang.value.harga = ''
    barang.value.stok = ''
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