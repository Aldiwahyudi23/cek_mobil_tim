<template>
  <div class="bg-white p-2 rounded-lg shadow-lg space-y-4">
 <!-- STNK File Input Section -->
    <div @click="openFileOptions" class="stnk-upload-box">
      <img v-if="uploadedImage" :src="uploadedImage" alt="Uploaded Image" class="thumbnail" />
      <p v-else>Upload STNK</p>
    </div>

    <!-- File Options Overlay -->
    <div v-if="showOverlay" class="overlay">
      <div class="overlay-content">
        <button @click="openCamera">Kamera</button>
        <button @click="openGallery">Galeri</button>
      </div>
      <button @click="closeOverlay" class="close-button">X</button>
    </div>

    <!-- Image Preview Page -->
    <div v-if="showPreviewPage" class="preview-page">
      <img :src="previewImage" alt="Preview Image" class="preview-image" />
      <button @click="cancelUpload">Batal</button>
      <button @click="saveImage">Simpan</button>
    </div>

 <!-- Overlay untuk Kamera dan Galeri -->
    <div v-if="showOverlay" class="fixed inset-0 bg-black bg-opacity-50 z-20 flex items-end justify-center" @click.self="showOverlay = false">
      <div class="overlay-content relative">
        <button @click="showOverlay = false" class="absolute top-2 right-2 text-gray-500 hover:text-gray-700">
          <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
        </button>
        <div class="flex space-x-4">
          <button @click="triggerFileInput('camera')" class="flex-1 py-2 px-4 bg-blue-500 text-white rounded-md">Kamera</button>
          <button @click="triggerFileInput('gallery')" class="flex-1 py-2 px-4 bg-blue-500 text-white rounded-md">Galeri</button>
        </div>
        <!-- Hidden file input -->
        <input type="file" ref="fileInput" style="display: none;" @change="handleFileChange" />
      </div>
    </div>

    <!-- Pajak -->
    <div>
      <label class="block text-gray-700">Pajak</label>
      <input v-model="pajak" type="date" class="w-full p-2 border rounded-md"/>
    </div>

    <!-- Pajak 5 tahunan -->
    <div>
      <label class="block text-gray-700">Pajak 5 Tahunan</label>
      <input v-model="pajak5Tahunan" type="date" class="w-full p-2 border rounded-md"/>
    </div>

    <!-- PKB -->
    <div>
      <label class="block text-gray-700">PKB</label>
      <input v-model="pkb" @input="formatCurrency" type="text" placeholder="PKB" class="w-full p-2 border rounded-md"/>
    </div>

    <!-- No Rangka -->
    <div>
      <label class="block text-gray-700">No Rangka</label>
      <input v-model="noRangka" @input="validateNoRangka" type="text" placeholder="No Rangka" class="w-full p-2 border rounded-md uppercase" maxlength="17"/>
    </div>

    <!-- No Mesin -->
    <div>
      <label class="block text-gray-700">No Mesin</label>
      <input v-model="noMesin" @input="validateNoMesin" type="text" placeholder="No Mesin" class="w-full p-2 border rounded-md uppercase" maxlength="17"/>
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
      showOverlay: false,
      selectedFile: null,
      showPreviewPage: false,
      uploadedImage: null, // Gambar yang diunggah
      previewImage: null, // Gambar yang akan ditampilkan di preview page
      pajak: '',
      pajak5Tahunan: '',
      pkb: '',
      noRangka: '',
      noMesin: '',
      // other data properties...
    };
  },
  computed: {
    isFormComplete() {
      // Mengembalikan true jika semua input terisi
      return this.pajak && this.pajak5Tahunan && this.pkb && this.noRangka && this.noMesin;
    },
    filledInputs() {
      // Menghitung jumlah input yang terisi
      return [
        this.pajak, this.pajak5Tahunan, this.pkb, 
        this.noRangka, this.noMesin
      ].filter(value => value).length;
    },
    remainingInputs() {
      // Menghitung jumlah input yang belum terisi
      return 5 - this.filledInputs;
    }
  },
  methods: {
 openFileOptions() {
      this.showOverlay = true;
    },
triggerFileInput(source) {
      // Trigger the hidden file input
      const fileInput = this.$refs.fileInput;
      if (source === 'camera') {
        // Open camera
        fileInput.accept = 'image/*';
      } else if (source === 'gallery') {
        // Open gallery
        fileInput.accept = 'image/*';
      }
      fileInput.click();
    },
    handleFileChange(event) {
      const file = event.target.files[0];
      if (file) {
        // Handle the file upload or display
        console.log("File selected:", file);
        this.selectedFile = file;
        // You can implement file upload logic here
      }
      this.showOverlay = false; // Close the overlay after file is selected
    },

    openCamera() {
      // Implementasikan logika untuk membuka kamera
      console.log("Buka Kamera");
      this.selectImage("camera");
      this.showOverlay = false;
    },
    openGallery() {
      // Implementasikan logika untuk membuka galeri
      console.log("Buka Galeri");
      this.selectImage("gallery");
      this.showOverlay = false;
    },
    closeOverlay() {
      this.showOverlay = false;
    },
    selectImage(source) {
      // Logika untuk memilih gambar dari kamera atau galeri
      // Misalnya, gunakan HTML5 API atau plugin khusus untuk memilih gambar
      // Setelah gambar dipilih:
      this.previewImage = "path/to/selected/image.jpg"; // Ubah dengan path gambar yang dipilih
      this.showPreviewPage = true;
    },
    cancelUpload() {
      this.showPreviewPage = false;
      this.previewImage = null;
    },
    saveImage() {
      // Logika untuk mengunggah gambar ke server
      console.log("Menyimpan gambar ke database...");
      // Simpan gambar ke server (latar belakang)
      this.uploadImageToServer(this.previewImage);
    },
    uploadImageToServer(image) {
      // Contoh penggunaan axios untuk mengunggah gambar
      axios.post('/upload-endpoint', { image })
        .then(response => {
          console.log("Gambar berhasil diunggah");
          this.uploadedImage = image; // Tampilkan gambar kecil di halaman utama
          this.showPreviewPage = false;
        })
        .catch(error => {
          console.error("Gagal mengunggah gambar:", error);
        });
    },
  
     
    formatCurrency(event) {
      const value = event.target.value.replace(/[^0-9]/g, '');
      event.target.value = new Intl.NumberFormat('id-ID').format(value);
      this.pkb = event.target.value;
    },
    validateNoRangka(event) {
      const value = event.target.value.toUpperCase().replace(/[^A-Z0-9]/g, '');
      event.target.value = value.slice(0, 17);
      this.noRangka = event.target.value;
    },
    validateNoMesin(event) {
      const value = event.target.value.toUpperCase().replace(/[^A-Z0-9]/g, '');
      event.target.value = value.slice(0, 17);
      this.noMesin = event.target.value;
    },
    goToNextPage() {
      // Logika untuk mengarahkan ke halaman berikutnya
      this.$router.push('/halaman-berikutnya');
    }
  }
};
</script>

<style scoped>
/* CSS untuk overlay */
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: 20;
}
.overlay-content {
  position: absolute;
  bottom: 0;
  width: 100%;
  max-width: 600px;
  background: white;
  border-radius: 8px 8px 0 0;
  padding: 16px;
}
</style>
<style>
.stnk-upload-box {
  border: 2px dashed #ccc;
  padding: 20px;
  text-align: center;
  cursor: pointer;
}
.overlay {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.5);
  padding: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.overlay-content {
  display: flex;
  gap: 10px;
}
.close-button {
  position: absolute;
  top: 10px;
  right: 10px;
  font-size: 20px;
  background: none;
  border: none;
  color: white;
  cursor: pointer;
}
.preview-page {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.preview-image {
  max-width: 80%;
  max-height: 80%;
}
.thumbnail {
  max-width: 100px;
  max-height: 100px;
}
</style>
