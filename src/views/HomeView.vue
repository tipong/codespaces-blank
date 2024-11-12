<template>
  <div id="app">
    <h1>Perhitungan IPK Mahasiswa</h1>
    <div>
      <label for="nim">NIM:</label>
      <input v-model="nim" id="nim" type="text" placeholder="Masukkan NIM" />
      
      <button @click="getIPK">Hitung IPK</button>
    </div>

    <div v-if="ipk !== null">
      <h2>IPK Mahasiswa: {{ ipk }}</h2>
    </div>

    <div v-if="errorMessage">
      <h3 style="color: red;">{{ errorMessage }}</h3>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      nim: "", // Input NIM mahasiswa
      ipk: null, // Hasil IPK yang diterima dari API
      errorMessage: "", // Pesan error jika terjadi kesalahan
    };
  },
  methods: {
    async getIPK() {
      if (!this.nim) {
        this.errorMessage = "NIM tidak boleh kosong!";
        return;
      }

      try {
        this.errorMessage = ""; // Menghapus pesan error
        const response = await fetch(`https://8000-idx-hitung-ipk-1731295467948.cluster-qpa6grkipzc64wfjrbr3hsdma2.cloudworkstations.dev/ipk/${this.nim}`, {
          method: "GET",
          headers: {
            "Content-Type": "application/json",
          },
        });

        if (response.ok) {
          const data = await response.json();
          this.ipk = data.ipk; // Mengambil nilai IPK dari response
        } else {
          this.errorMessage = "Gagal mengambil data IPK. Pastikan NIM valid.";
        }
      } catch (error) {
        this.errorMessage = "Terjadi kesalahan dalam menghubungi API.";
        console.error("Error:", error);
      }
    },
  },
};
</script>