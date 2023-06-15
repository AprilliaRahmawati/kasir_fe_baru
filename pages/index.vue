<template>
  <div class="container">

    <div class="flex flex-col items-center">
      <h3 class="text-2xl font-semibold mb-4">Masukkan Kode Barang</h3>
      <div class="flex items-center space-x-4">
        <input
          type="text"
          placeholder="Kode Barang"
          class="border border-gray-300 px-4 py-2 rounded"
        />
        <button
          class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
        >
          Cari
        </button>
      </div>
    </div>

    <div class="flex flex-col items-center">
      <h3 class="text-2xl font-semibold mb-4">Scan Barcode</h3>
      <div class="flex items-center space-x-4">
        <!-- Konten scan barcode -->
      </div>
    </div>

    <h3 class="text-2xl font-semibold mb-4">Keranjang</h3>
    <table class="min-w-full divide-y divide-gray-200">
      <thead class="bg-gray-50">
        <tr>
          <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Nama</th>
          <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Harga</th>
          <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Qty</th>
          <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Sub Total</th>
          <th scope="col" class="relative px-6 py-3"></th>
        </tr>
      </thead>
      <tbody class="bg-white divide-y divide-gray-200">
        <!-- Isi data keranjang di sini -->
        <tr>
          <td class="px-6 py-4 whitespace-nowrap">Product 1</td>
          <td class="px-6 py-4 whitespace-nowrap">Rp 10.000</td>
          <td class="px-6 py-4 whitespace-nowrap">2</td>
          <td class="px-6 py-4 whitespace-nowrap">Rp 20.000</td>
          <td class="px-6 py-4 whitespace-nowrap">
            <button class="text-red-600 hover:text-red-900">Delete</button>
          </td>
        </tr>
        <tr>
          <td class="px-6 py-4 whitespace-nowrap">Product 2</td>
          <td class="px-6 py-4 whitespace-nowrap">Rp 15.000</td>
          <td class="px-6 py-4 whitespace-nowrap">3</td>
          <td class="px-6 py-4 whitespace-nowrap">Rp 45.000</td>
          <td class="px-6 py-4 whitespace-nowrap">
            <button class="text-red-600 hover:text-red-900">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>

    <li>
      <NuxtLink to="/resetkeranjang" class="menu-link">
        <span class="menu-icon">🔄</span>
        <span class="menu-text">Reset Keranjang</span>
      </NuxtLink>
    </li>

    <h4 class="text-2xl font-semibold mb-4">Total Pembayaran = </h4>

    <br />

    <button class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">
      Bayar
    </button>
  </div>
</template>

<style scoped>
.menu-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.menu-link {
  display: flex;
  align-items: center;
  text-decoration: none;
  color: #333;
  padding: 10px;
  margin-bottom: 10px;
  background-color: #f0f0f0;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}

.menu-link:hover {
  background-color: #ddd;
}

.menu-icon {
  margin-right: 10px;
}

.menu-text {
  font-weight: bold;
}
</style>
