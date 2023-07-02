<template>
    <div class="container mx-auto xl:px-20 my-8">
      <h1 class="text-4xl font-bold">Edit Barang</h1>
      <form @submit="updateProduct">
        <div class="mt-8">
          <label for="name" class="block mb-2">Nama</label>
          <input v-model="product.name" type="text" id="name" class="border px-4 py-2 rounded" required>
        </div>
        <div class="mt-4">
          <label for="price" class="block mb-2">Harga</label>
          <input v-model="product.price" type="number" id="price" class="border px-4 py-2 rounded" required>
        </div>
        <div class="mt-4">
          <label for="stock" class="block mb-2">Stok</label>
          <input v-model="product.stock" type="number" id="stock" class="border px-4 py-2 rounded" required>
        </div>
        <div class="mt-4">
          <label for="code" class="block mb-2">Kode</label>
          <input v-model="product.code" type="text" id="code" class="border px-4 py-2 rounded" required>
        </div>
        <div class="mt-8">
          <button type="submit" class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded">
            Simpan Perubahan
          </button>
        </div>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        product: {
          id: '',
          name: '',
          price: '',
          stock: '',
          code: '',
        },
      };
    },
    async mounted() {
      // Ambil data barang yang akan diedit menggunakan API
      const productId = this.$route.params.id;
      try {
        const response = await this.$axios.get(`/api/product/${productId}`);
        this.product = response.data;
      } catch (error) {
        console.error(error);
        // Tambahkan penanganan kesalahan jika diperlukan
      }
    },
    methods: {
      async updateProduct() {
        try {
          const response = await this.$axios.put(`/api/product/${this.product.id}`, this.product);
          // Lakukan penanganan setelah pembaruan produk berhasil, jika diperlukan
          // Misalnya, menampilkan pesan sukses atau mengarahkan pengguna ke halaman lain
          console.log('Produk berhasil diperbarui', response.data);
        } catch (error) {
          console.error(error);
          // Tambahkan penanganan kesalahan jika diperlukan
        }
      },
    },
  };
  </script>
  