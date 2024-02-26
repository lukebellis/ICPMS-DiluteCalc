<template>
  <div class="min-h-screen flex flex-col items-center justify-center bg-gradient-to-r from-green-400 to-blue-500">
    <div class="text-center text-white px-4 sm:px-6 lg:px-8">
      <h1 class="text-5xl font-bold mb-4">ICP-MS Dilution Calculator</h1>
      <p class="mb-8 text-xl">Effortlessly calculate dilutions for ICP-MS analysis, optimising your sample preparation process.</p>
    </div>
    <button v-if="!showCalculator" @click="showCalculator = true" class="bg-white text-green-500 font-bold py-2 px-4 rounded transition duration-300 ease-in-out transform hover:-translate-y-1 hover:scale-110">
      Start Calculating
    </button>
    <DilutionForm v-if="showCalculator" class="w-full max-w-xl mt-8" />
    <!-- Footer with GitHub link -->
    <div class="mt-8">
      <p class="text-white text-center">
        Made by Luke Ellis
        <a href="https://github.com/lukebellis" target="_blank" class="text-green-300 hover:text-green-500">
          <i class="fab fa-github"></i> GitHub
        </a>
      </p>
    </div>
  </div>
</template>

  
  <script>
  import DilutionForm from './DilutionForm.vue';
  
  export default {
    components: {
      DilutionForm,
    },
    data() {
      return {
        showCalculator: false,
      };
    },
  };
  </script>
  