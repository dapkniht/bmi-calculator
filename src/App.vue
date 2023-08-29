<template>
  <div class="container m-auto min-h-screen font-poppins">
    <div class="p-2 mt-4 w-1/2 mx-auto border-2 shadow rounded bg-slate-100">
      <h2 class="text-center text-2xl text-slate-800 my-4">BMI Calculator</h2>
      <div class="my-4">
        <input
          type="range"
          min="1"
          max="999"
          v-model="weight"
          class="w-full my-2"
        />
        <div class="flex gap-4">
          <p>Weight:</p>
          <div>
            <span><input type="text" class="w-10" v-model="weight" /> Kg</span>
          </div>
        </div>
      </div>
      <div class="my-10">
        <input
          type="range"
          min="1"
          max="999"
          v-model="height"
          class="w-full my-2"
        />
        <div class="flex gap-4">
          <p>Height:</p>
          <div>
            <span><input type="text" class="w-10" v-model="height" /> Cm</span>
          </div>
        </div>
      </div>
      <div class="flex justify-between">
        <p class="font-bold">
          Your BMI: <span class="font-normal">{{ bmi }}</span>
        </p>
        <p :class="[status.color, 'p-2 rounded']">
          {{ status.classification }}
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed, watch } from "vue";
const weight = ref(65);
const height = ref(170);
const status = reactive({
  classification: "",
  color: "",
});

const bmi = computed(() => {
  const value = weight.value / Math.pow(height.value / 100, 2);
  classification(value);
  return value.toFixed(1);
});

watch(weight, (newValue, oldValue) => {
  if (weight.value > 999 || weight.value / 1 != weight.value)
    weight.value = oldValue;
});
watch(height, (newValue, oldValue) => {
  if (height.value > 999 || height.value / 1 != height.value)
    height.value = oldValue;
});

const classification = (range) => {
  if (range < 16) {
    status.classification = "Severe Thinness";
    status.color = "bg-red-500";
  } else if (range >= 16 && range <= 17) {
    status.classification = "Moderate Thinness";
    status.color = "bg-red-400";
  } else if (range >= 17 && range <= 18.5) {
    status.classification = "Mild Thinness";
    status.color = "bg-yellow-500";
  } else if (range >= 18.5 && range <= 25) {
    status.classification = "Normal";
    status.color = "bg-green-500";
  } else if (range >= 25 && range <= 30) {
    status.classification = "Overweight";
    status.color = "bg-yellow-500";
  } else if (range >= 30 && range <= 35) {
    status.classification = "Obese Class I";
    status.color = "bg-red-400";
  } else if (range >= 35 && range <= 40) {
    status.classification = "Obese Class II";
    status.color = "bg-red-500";
  } else if (range > 40) {
    status.classification = "Obese Class III";
    status.color = "bg-red-600";
  }
};
</script>
