<script setup>
  const config = useRuntimeConfig();
  const { $swal } = useNuxtApp();
  const isLoading = ref(false);
  const product = ref([]);
  const errors = ref({});
  getBarang();

  async function getBarang() {
    isLoading.value = true;
    try {
      const response = await $fetch(`${config.public.apiBase}/api/product`);
      console.log(response);
      product.value = response;
    } catch (error) {
      console.log(error);
      console.log(error.data);
      if (error.data.status === 400) {
        errors.value = error.data.errors;
      }
    }
    isLoading.value = false;
  }

  async function deleteBarang(id) {
  isLoading.value = true
  try {
    const response = await $fetch(`${config.public.apiBase}/api/product/`+id, {
      method: 'DELETE',
      headers: {
        'Accept': 'application/json',
      }
    })
    //berfungsi untuk mentracking data
    console.log(response)

    $swal.fire({
      icon: 'success',
      title: 'Sukses',
      text: 'Data barang berhasil dihapus.',
    })
    getBarang();
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

<template>
  <div class="container mx-auto xl:px-20 my-8">
    <div class="flex justify-between">
      <h1 class="text-4xl font-bold">Data Barang</h1>
      <NuxtLink
        to="/barang/create"
        class="flex-none bg-blue-700 px-4 py-2 rounded-lg text-white hover:bg-blue-800 transition duration-150"
      >
        Tambah
      </NuxtLink>
    </div>
    <div class="mt-8">
      <table class="w-full text-center table-auto shadow-md">
        <thead class="bg-slate-700 text-white">
          <tr>
            <th scope="col" class="px-6 py-3">Nama</th>
            <th scope="col" class="px-6 py-3">Harga</th>
            <th scope="col" class="px-6 py-3">Stok</th>
            <th scope="col" class="px-6 py-3">Kode</th>
            <th scope="col" class="px-6 py-3">Aksi</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="product in product.data" :key="product.id" class="border-b">
            <td class="px-6 py-4">{{ product.name }}</td>
            <td class="px-6 py-4">{{ product.price }}</td>
            <td class="px-6 py-4">{{ product.stock }}</td>
            <td class="px-6 py-4">{{ product.code }}</td>
            <td class="px-6 py-4">
              <NuxtLink
              to="/barang/edit"
              class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded">
              Edit
            </NuxtLink>
              <button @click="deleteBarang(product.id)" class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded ml-2">
                Hapus
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
