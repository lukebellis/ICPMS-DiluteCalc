<template>
    <div class="max-w-md mx-auto mt-5 sm:mt-10 bg-white p-4 sm:p-8 rounded-lg shadow-lg">
      <form @submit.prevent="calculateDilution" class="space-y-3 sm:space-y-4">
        <div class="relative flex flex-col sm:flex-row items-center space-y-3 sm:space-y-0 sm:space-x-2">
          <input type="number" id="measuredConcentration" v-model.number="measuredConcentration" class="input flex-auto sm:pr-24" placeholder="Measured Concentration" required>
          <select v-model="measuredUnit" class="unit-select w-full sm:w-24 sm:absolute sm:inset-y-0 sm:right-0">
            <option value="ug/L">µg/L</option>
            <option value="mg/L">mg/L</option>
            <option value="ng/L">ng/L</option>
          </select>
        </div>
        <div class="relative flex flex-col sm:flex-row items-center space-y-3 sm:space-y-0 sm:space-x-2">
          <input type="number" id="limitOfDetection" v-model.number="limitOfDetection" class="input flex-auto sm:pr-24" placeholder="Limit of Detection (LoD)" required>
          <select v-model="lodUnit" class="unit-select w-full sm:w-24 sm:absolute sm:inset-y-0 sm:right-0">
            <option value="ug/L">µg/L</option>
            <option value="mg/L">mg/L</option>
            <option value="ng/L">ng/L</option>
          </select>
        </div>
        <input type="number" id="sampleVolume" v-model.number="sampleVolume" class="input w-full" placeholder="Sample Volume (mL)" required>
        <button type="submit" class="btn w-full bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-2 sm:py-3 px-4 rounded focus:outline-none focus:ring-2 focus:ring-emerald-500 focus:ring-opacity-50">
          Calculate
        </button>
      </form>
      <div v-if="dilutionResult" class="mt-6 result">
        <p>Required Dilution Factor: {{ dilutionResult.factor.toFixed(2) }}</p>
        <p>This is a x{{ dilutionResult.factor.toFixed(0) }} dilution.</p>
        <p>Math involved: {{ dilutionResult.description }}</p>
        <p v-if="analyteVolume">Volume of sample to use: {{ analyteVolume.toFixed(2) }} mL</p>
        <p v-if="solventVolume">Volume of solvent to add: {{ solventVolume.toFixed(2) }} mL</p>
      </div>
    </div>
  </template>
  
  
  
  
  
  <script>
  export default {
    data() {
      return {
        measuredConcentration: null,
        measuredUnit: 'ug/L', // Default unit for measured concentration
        limitOfDetection: null,
        lodUnit: 'ug/L', // Default unit for limit of detection
        sampleVolume: null, // User input for the total volume of the sample
        dilutionResult: null, // To store the dilution factor and description
        analyteVolume: null, // Calculated volume of analyte to use
        solventVolume: null, // Calculated volume of solvent to add
      };
    },
    methods: {
      calculateDilution() {
        const measuredInUgL = this.convertToMicrogramsPerLiter(this.measuredConcentration, this.measuredUnit);
        const lodInUgL = this.convertToMicrogramsPerLiter(this.limitOfDetection, this.lodUnit);
        
        // Initial calculation of the dilution factor
        let factor = measuredInUgL / lodInUgL;
        
        // Rounding the factor to the nearest whole number
        factor = Math.ceil(factor);
  
        // Updating the dilutionResult object with the rounded factor and a descriptive message
        this.dilutionResult = {
          factor,
          description: `To bring a concentration of ${this.measuredConcentration} ${this.measuredUnit} to within the limit of detection (${this.limitOfDetection} ${this.lodUnit}), a dilution factor of ${factor} is required.`,
        };
    
        // Calculating volumes using the rounded factor
        this.calculateVolumes(factor);
      },
      convertToMicrogramsPerLiter(concentration, unit) {
        switch (unit) {
          case 'mg/L': return concentration * 1000; // Convert mg/L to µg/L
          case 'ng/L': return concentration / 1000; // Convert ng/L to µg/L
          default: return concentration; // Assume µg/L by default
        }
      },
      calculateVolumes(factor) {
        if (this.sampleVolume && factor) {
          // Calculate the volume of analyte to use based on the rounded dilution factor
          this.analyteVolume = this.sampleVolume / factor;
          
          // Calculate the volume of solvent to add to reach the sample volume
          this.solventVolume = this.sampleVolume - this.analyteVolume;
        }
      },
    },
  };
  </script>
  
  

  
  
  <style scoped>
  .input {
    @apply appearance-none block w-full bg-gray-100 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500;
    @apply shadow-md; /* Add shadow to the input */
  }
  
  .unit-select {
    @apply border border-gray-200 text-gray-700 py-3 px-4 rounded shadow-md; /* Ensure the select has the same styling */
    @apply bg-gray-100 cursor-pointer; /* Additional styling for select */
  }
  
  .btn {
    @apply bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-lg shadow;
  }
  
  </style>
  