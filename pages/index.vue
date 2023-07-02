<script setup>
import Quagga from 'quagga';
const keranjang = ref([]);
const barcode = ref('');

const config = useRuntimeConfig();
const { data: product } = await useFetch(`${config.public.apiBase}/api/product`);

const resetKeranjang = () => {
  keranjang.value = [];
};

const tambahKeranjang = (code) => {
  const rowdata = ref(product.value.data.find(product => product.code === code));

  if(!rowdata.value) return;

  keranjang.value.push({
    barcode: rowdata.value.code,
    name: rowdata.value.name,
    price: rowdata.value.price,
    qty: 1,
    subtotal: rowdata.value.price * 1
  });

  barcode.value = '';
};

const hapusBarang = (index) => {
  keranjang.value.splice(index, 1);
};

const updateQuantity = (index, newQty) => {
  keranjang.value[index].qty = newQty;
  keranjang.value[index].subtotal = keranjang.value[index].price * newQty;
};

const hitungTotalPembayaran = () => {
  let total = 0;
  keranjang.value.forEach(item => {
    total += item.subtotal;
  });
  return total;
};

const nyalakanKamera = () => {
  // Periksa apakah browser mendukung WebRTC (kamera)
  if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
    alert('Browser tidak mendukung akses kamera.');
    return;
  }

  // Dapatkan akses ke kamera pengguna
  navigator.mediaDevices.getUserMedia({ video: true })
    .then(handleKameraSuccess)
    .catch(handleKameraError);
};

const handleKameraSuccess = (stream) => {
  // Dapatkan elemen video untuk menampilkan tampilan kamera
  const videoElement = document.getElementById('kamera');

  // Putar tampilan kamera dalam elemen video
  videoElement.srcObject = stream;

  // Inisialisasi QuaggaJS dengan konfigurasi pemindaian barcode
  Quagga.init({
    inputStream: {
      constraints: {
        video: true,
      },
      target: videoElement,
    },
    decoder: {
      readers: ['ean_reader'], // Pilih jenis barcode yang ingin kamu pindai, seperti EAN.
    },
  }, function (err) {
    if (err) {
      console.error('Error initializing Quagga:', err);
      return;
    }
    console.log('Quagga initialized.');
    Quagga.start();
  });

  // Tangkap hasil pemindaian barcode
  Quagga.onDetected(function (result) {
    console.log(result);
    const barcode = result.codeResult.code;
    tambahKeranjang(barcode);

    // Tampilkan informasi produk di keranjang berdasarkan barcode
    const rowdata = product.value.data.find((product) => product.code === barcode);
    if (rowdata) {
      keranjang.value.push({
        barcode: rowdata.code,
        name: rowdata.name,
        price: rowdata.price,
        qty: 1,
        subtotal: rowdata.price * 1,
      });
    }
  });
};


</script>

<template>
  <div class="container">
    <div class="flex flex-col items-center">
      <h3 class="text-2xl font-semibold mb-4">Masukkan Kode Barang</h3>
      <div class="flex items-center space-x-4">
        <input
          v-model="barcode"
          @keyup.enter="tambahKeranjang(barcode)"
          type="text"
          placeholder="Kode Barang"
          class="border border-gray-300 px-4 py-2 rounded"
        />
      </div>
    </div> <br>

    <div class="flex flex-col items-center">
      <h3 class="text-2xl font-semibold mb-4">Scan Barcode</h3>
      <button class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600" @click="nyalakanKamera">Nyalakan Kamera</button>
      <div class="flex items-center space-x-4">
        <!-- Konten scan barcode -->
      </div>
      <div class="flex items-center space-x-4">
        <video id="kamera" autoplay></video>
      </div>
    </div>


    <div class="grid grid-cols-3 gap-4">
      <div class="p-4 col-span-2">
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
            <tr v-for="(keranjang, index) in keranjang" :key="keranjang.id">
              <td class="px-6 py-4 whitespace-nowrap">{{ keranjang.name }}</td>
              <td class="px-6 py-4 whitespace-nowrap">{{ keranjang.price }}</td>
              <td class="px-6 py-4 whitespace-nowrap">
                <input type="number" v-model="keranjang.qty" @change="updateQuantity(index, keranjang.qty)">
              </td>
              <td class="px-6 py-4 whitespace-nowrap">{{ keranjang.subtotal }}</td>
              <td class="px-6 py-4 whitespace-nowrap">
                <button class="text-red-600 hover:text-red-900" @click="hapusBarang(index)">Delete</button>
              </td>
            </tr>
          </tbody>
        </table>

        <button
          class="bg-blue-500 text-white px-4 py-2 mt-4 rounded hover:bg-blue-600"
          @click="resetKeranjang"
        >
          Reset Keranjang
        </button>

      </div>
      <div class="p-4">
        <h4 class="text-2xl font-semibold mb-4">Total Pembayaran = {{ hitungTotalPembayaran() }}</h4>

        <br />

        <button class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">
          Bayar
        </button>
      </div>
    </div>
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

