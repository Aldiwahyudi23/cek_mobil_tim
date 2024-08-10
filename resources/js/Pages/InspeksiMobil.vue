<template>
  <div class="min-h-screen bg-gray-100">
    <!-- Sticky Navigation -->
    <div class="bg-blue-500 text-white fixed top-0 w-full shadow-md z-10">
      <div class="max-w-md mx-auto p-2">
        <h1 class="text-2xl font-bold">Inspeksi Mobil</h1>
      </div>
    </div>

    <!-- Sticky Kategori Menu Horizontal -->
    <div class="bg-white shadow-md fixed top-12 w-full z-10">
      <div class="max-w-md mx-auto flex overflow-x-auto whitespace-nowrap no-scrollbar">
        <button v-for="(kategori, index) in categories" :key="index" 
                @click="selectCategory(index)"
                class="py-2 px-4 text-gray-700 hover:bg-blue-500 hover:text-white transition duration-300" 
                :class="{ 'bg-blue-500 text-white': selectedCategory === index }">
          {{ kategori.name }}
        </button>
      </div>
    </div>

    <!-- Konten Kategori -->
    <div class="max-w-md mx-auto mt-20 p-4 bg-white shadow-md rounded-lg">
      <component :is="categories[selectedCategory].component"></component>
    </div>
  </div>
</template>

<script>
import tipe from './inspek/tipe.vue';  // Komponen untuk kategori Tipe
import Dokumen from './inspek/Dokumen.vue';  // Komponen untuk kategori Dokumen
import Mesin from './inspek/Mesin.vue';  // Komponen untuk kategori Mesin
import Interior from './inspek/Interior.vue';  // Komponen untuk kategori Interior
import Eksterior from './inspek/Eksterior.vue';  // Komponen untuk kategori Eksterior

export default {
  data() {
    return {
      selectedCategory: 0, // Default kategori pertama yang dipilih
      categories: [
        { name: 'Tipe ', component: tipe },
        { name: 'Dokumen', component: Dokumen },
        { name: 'Mesin', component: Mesin },
        { name: 'Interior', component: Interior },
        { name: 'Eksterior', component: Eksterior },
      ],
    };
  },
  methods: {
    selectCategory(index) {
      this.selectedCategory = index;
    }
  }
};
</script>

<style>
/* Menghilangkan scrollbar untuk Chrome, Safari, Opera */
.no-scrollbar::-webkit-scrollbar {
  display: none;
}

/* Menghilangkan scrollbar untuk IE, Edge, dan Firefox */
.no-scrollbar {
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;  /* Firefox */
}
</style>
