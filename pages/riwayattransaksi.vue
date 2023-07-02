<template>
  <div class="container mx-auto xl:px-20 my-8">
    <h1 class="text-4xl font-bold">Riwayat Transaksi</h1>
    <div class="mt-8">
      <table class="w-full text-center table-auto shadow-md">
        <thead class="bg-slate-700 text-white">
          <tr>
            <th scope="col" class="px-6 py-3">Nomor Transaksi</th>
            <th scope="col" class="px-6 py-3">Tanggal</th>
            <th scope="col" class="px-6 py-3">Total</th>
            <th scope="col" class="px-6 py-3">Aksi</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="transaction in transactions" :key="transaction.id" class="border-b">
            <td class="px-6 py-4">{{ transaction.number }}</td>
            <td class="px-6 py-4">{{ formatDate(transaction.date) }}</td>
            <td class="px-6 py-4">{{ formatCurrency(transaction.total) }}</td>
            <td class="px-6 py-4">
              <NuxtLink
                :to="`/transaksi/${transaction.id}`"
                class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded"
              >
                Lihat
              </NuxtLink>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      transactions: [],
    };
  },
  created() {
    this.fetchTransactions();
  },
  methods: {
    async fetchTransactions() {
      try {
        const response = await fetch('/api/transactions');
        if (response.ok) {
          this.transactions = await response.json();
        } else {
          throw new Error('Gagal mengambil riwayat transaksi');
        }
      } catch (error) {
        console.error(error);
        // Tambahkan penanganan kesalahan jika diperlukan
      }
    },
    formatDate(date) {
      // Logika untuk memformat tanggal, sesuaikan dengan format yang Anda inginkan
    },
    formatCurrency(amount) {
      // Logika untuk memformat jumlah uang, sesuaikan dengan format yang Anda inginkan
    },
  },
};
</script>
