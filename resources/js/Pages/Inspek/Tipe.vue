<template>
  <div class="bg-white p-2 rounded-lg shadow-lg space-y-4">
    <!-- Nopol -->
    <div class="flex space-x-2">
      <input
        type="text"
        placeholder="D"
        v-model="nopolD"
        @input="validateLetters($event, 2)"
        class="w-1/5 p-2 border rounded-md uppercase"
        maxlength="2"
      />
      <input
        type="text"
        placeholder="1234"
        v-model="nopolNumber"
        @input="validateNumbers($event, 4)"
        class="w-1/2 p-2 border rounded-md"
        maxlength="4"
      />
      <input
        type="text"
        placeholder="XY"
        v-model="nopolXY"
        @input="validateLetters($event, 3)"
        class="w-1/3 p-2 border rounded-md uppercase"
        maxlength="3"
      />
    </div>

    <!-- Merek -->
    <div>
      <label class="block text-gray-700">Merek</label>
      <select v-model="merek" class="w-full p-2 border rounded-md">
        <option value="">Pilih Merek</option>
        <option value="toyota">Toyota</option>
        <option value="honda">Honda</option>
        <option value="suzuki">Suzuki</option>
      </select>
    </div>

    <!-- Tahun Pembuatan -->
    <div>
      <label class="block text-gray-700">Tahun Pembuatan</label>
      <input v-model="tahunPembuatan" type="number" placeholder="Tahun Pembuatan" class="w-full p-2 border rounded-md" />
    </div>

    <!-- Model -->
    <div>
      <label class="block text-gray-700">Model</label>
      <select v-model="model" class="w-full p-2 border rounded-md">
        <option value="">Pilih Model</option>
        <!-- Tambahkan pilihan model -->
      </select>
    </div>

    <!-- Type -->
    <div>
      <label class="block text-gray-700">Type</label>
      <select v-model="type" class="w-full p-2 border rounded-md">
        <option value="">Pilih Type</option>
        <!-- Tambahkan pilihan type -->
      </select>
    </div>

    <!-- Kapasitas (CC) -->
    <div>
      <label class="block text-gray-700">Kapasitas (CC)</label>
      <select v-model="kapasitasCC" class="w-full p-2 border rounded-md">
        <option value="">Pilih Kapasitas</option>
        <!-- Tambahkan pilihan kapasitas -->
      </select>
    </div>

    <!-- Transmisi -->
    <div>
      <label class="block text-gray-700">Transmisi</label>
      <div class="flex space-x-4">
        <button @click="transmisi = 'AT'"
                :class="{'bg-blue-500 text-white': transmisi === 'AT', 'bg-gray-200': transmisi !== 'AT'}"
                class="w-1/2 p-2 rounded-md">
          AT
        </button>
        <button @click="transmisi = 'MT'"
                :class="{'bg-blue-500 text-white': transmisi === 'MT', 'bg-gray-200': transmisi !== 'MT'}"
                class="w-1/2 p-2 rounded-md">
          MT
        </button>
      </div>
    </div>

    <!-- Bahan Bakar -->
    <div>
      <label class="block text-gray-700">Bahan Bakar</label>
      <select v-model="bahanBakar" class="w-full p-2 border rounded-md">
        <option value="">Pilih Bahan Bakar</option>
        <!-- Tambahkan pilihan bahan bakar -->
      </select>
    </div>

    <!-- Periode Model -->
    <div>
      <label class="block text-gray-700">Periode Model</label>
      <select v-model="periodeModel" class="w-full p-2 border rounded-md">
        <option value="">Pilih Periode Model</option>
        <!-- Tambahkan pilihan periode model -->
      </select>
    </div>

    <!-- Warna -->
    <div>
      <label class="block text-gray-700">Warna</label>
      <input v-model="warna" type="text" placeholder="Warna" class="w-full p-2 border rounded-md uppercase" />
    </div>

    <!-- Tombol Aksi -->
    <div class="mt-6">
      <button
        @click="goToNextPage"
        :disabled="!isFormComplete"
        class="w-full p-2 bg-blue-500 text-white rounded-md disabled:bg-gray-400 disabled:cursor-not-allowed"
      >
        {{ filledInputs }} Input Terisi, {{ remainingInputs }} Belum Terisi
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      transmisi: '',
      nopolD: '',
      nopolNumber: '',
      nopolXY: '',
      merek: '',
      tahunPembuatan: '',
      model: '',
      type: '',
      kapasitasCC: '',
      bahanBakar: '',
      periodeModel: '',
      warna: ''
    };
  },
  computed: {
    isFormComplete() {
      // Mengembalikan true jika semua input terisi
      return this.nopolD && this.nopolNumber && this.nopolXY && 
             this.merek && this.tahunPembuatan && this.model && 
             this.type && this.kapasitasCC && this.bahanBakar && 
             this.periodeModel && this.warna && this.transmisi;
    },
    filledInputs() {
      // Menghitung jumlah input yang terisi
      return [
        this.nopolD, this.nopolNumber, this.nopolXY, 
        this.merek, this.tahunPembuatan, this.model, 
        this.type, this.kapasitasCC, this.bahanBakar, 
        this.periodeModel, this.warna, this.transmisi
      ].filter(value => value).length;
    },
    remainingInputs() {
      // Menghitung jumlah input yang belum terisi
      return 12 - this.filledInputs;
    }
  },
  methods: {
    validateLetters(event, maxLength) {
      const value = event.target.value.toUpperCase();
      event.target.value = value.replace(/[^A-Z]/g, '').slice(0, maxLength);
      this.$set(event.target, 'value', event.target.value);
    },
    validateNumbers(event, maxLength) {
      const value = event.target.value;
      event.target.value = value.replace(/[^0-9]/g, '').slice(0, maxLength);
      this.$set(event.target, 'value', event.target.value);
    },
    goToNextPage() {
      // Logika untuk mengarahkan ke halaman berikutnya
      // Misalnya, menggunakan router untuk navigasi
      this.$router.push('/mesin');
    }
  }
};
</script>

<style>
/* CSS untuk mengubah teks menjadi kapital secara otomatis */
.uppercase {
  text-transform: uppercase;
}
</style>
