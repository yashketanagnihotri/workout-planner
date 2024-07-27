<script setup>
import { ref, watch } from "vue";
import axios from "axios";
import ExcerciseDetails from "./components/ExcerciseDetails.vue";
import Navbar from "./components/Navbar.vue";

const apiData = ref(""); // Holds the fetched API data
const bodyPart = ref(""); // Holds the selected body part

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

// Function to fetch exercises based on selected body part
const fetchExercises = async () => {
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
    apiData.value = response.data; // Update the apiData ref with fetched data
    console.log(apiData.value); // Log the fetched data for debugging
  } catch (error) {
    console.error("Error fetching exercises:", error);
  }
};

// Watch for changes in bodyPart.value and fetch new exercises accordingly
watch(bodyPart, () => {
  if (bodyPart.value) {
    fetchExercises();
  }
});

// Function to update selected muscle when dropdown selection changes
const updateSelectedMuscle = (e) => {
  bodyPart.value = e.target.value; // Update bodyPart.value with the selected muscle
};
</script>

<template>
  <Navbar />
  <div class="container" id="app-container">
    <div class="flex justify-between">
      <h1 class="text-3xl mb-6">Choose the muscle 💪</h1>
      <select
        @change="updateSelectedMuscle"
        class="bg-slate-100 px-5 py-3 rounded-lg mb-5"
      >
        <option v-for="(muscle, index) in muscles" :value="muscle" :key="index">
          {{ muscle }}
        </option>
      </select>
    </div>
    <ExcerciseDetails :details="apiData" />
    <!-- <h1 v-if="bodyPart" v-for="names in apiData">{{ names.name }}</h1> -->
  </div>
</template>

<style scoped>
#app-container {
  padding: 5% 12%;
}
</style>
