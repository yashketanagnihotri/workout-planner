<script setup>
import { ref, watch, onMounted, onBeforeUnmount } from "vue";
import axios from "axios";
import ExcerciseDetails from "./components/ExcerciseDetails.vue";
import Navbar from "./components/Navbar.vue";
import * as THREE from "three";
import FOG from "vanta/dist/vanta.fog.min";

const apiData = ref(null); // Holds the fetched API data
const bodyPart = ref(""); // Holds the selected body part
const isLoading = ref(false); // Loading state indicator

const muscles = [
  "back",
  "cardio",
  "chest",
  "lower arms",
  "lower legs",
  "neck",
  "shoulders",
  "upper arms",
  "upper legs",
  "waist",
];

const fetchExercises = async () => {
  isLoading.value = true;
  try {
    const response = await axios.get(
      `https://exercisedb.p.rapidapi.com/exercises/bodyPart/${bodyPart.value}`,
      {
        headers: {
          "x-rapidapi-key":
            "4a163c275emsh186bfca3d170053p1b2b56jsnefb108ea58e8",
          "x-rapidapi-host": "exercisedb.p.rapidapi.com",
        },
      }
    );
    apiData.value = response.data;
    console.log(apiData.value);
  } catch (error) {
    console.error("Error fetching exercises:", error);
  } finally {
    isLoading.value = false;
  }
};

watch(bodyPart, () => {
  if (bodyPart.value) {
    fetchExercises();
  }
});

const updateSelectedMuscle = (e) => {
  bodyPart.value = e.target.value;
};

let vantaEffect = null;

onMounted(() => {
  vantaEffect = FOG({
    el: "#vanta-background",
    THREE: THREE,
    mouseControls: true,
    touchControls: true,
    gyroControls: true,
    minHeight: 200.0,
    minWidth: 200.0,
    highlightColor: 0xffa700,
    midtoneColor: 0xff2000,
    lowlightColor: 0x1400ff,
    baseColor: 0x2d00ff,
    blurFactor: 0.9,
    speed: 2.8,
    zoom: 0.4,
  });
});

onBeforeUnmount(() => {
  if (vantaEffect) vantaEffect.destroy();
});
</script>

<template>
  <div class="vanta-container">
    <div id="vanta-background"></div>
    <Navbar />
    <div class="content">
      <div class="flex justify-between">
        <h1
          class="md:text-3xl lg:text-3xl text-xl mb-6 text-slate-50 capitalize"
        >
          Choose the muscle
        </h1>
        <select
          @change="updateSelectedMuscle"
          class="bg-slate-100 px-5 py-3 rounded-lg mb-5"
        >
          <option
            v-for="(muscle, index) in muscles"
            :value="muscle"
            :key="index"
          >
            {{ muscle }}
          </option>
        </select>
      </div>
      <div v-if="isLoading">Loading...</div>
      <div v-else-if="apiData">
        <ExcerciseDetails :details="apiData" />
      </div>
      <div v-else class="text-slate-50">
        <p>Please select a muscle group to see exercises.</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.vanta-container {
  position: relative;
  height: 100%;
  min-height: 100vh; /* Ensure the container covers at least the viewport height */
  width: 100%;
  overflow: hidden; /* Ensure no overflow issues */
}

#vanta-background {
  position: fixed; /* Fixed to stay in place while scrolling */
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1; /* Ensure the Vanta background is behind other content */
}

.content {
  position: relative;
  z-index: 1; /* Ensure the content is above the Vanta background */
  padding: 5% 12%;
}
</style>
