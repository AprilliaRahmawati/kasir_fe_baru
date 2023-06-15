<template>
  <div class="container mx-auto xl:px-20 my-8">
    <div class="flex justify-between">
      <h1 class="text-4xl font-bold">Data Barang</h1>
      <NuxtLink
        to="/barang/create"
        class="flex-none bg-blue-700 px-4 py-2 rounded-lg text-white hover:bg-blue-800 transition duration-150"
        >Tambah</NuxtLink
      >
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
      </table>
    </div>
  </div>
</template>
