<template>
  <div
    class="flex justify-between bg-slate-100 mb-5 p-10 rounded-lg border-2 border-transparent hover:border-orange-500 focus:border-orange-500 cursor-pointer transition duration-300 ease-in-out"
    v-for="data in props.details"
  >
    <div>
      <h1 class="text-xl mb-5">{{ data.name }}</h1>
      <ol>
        <li
          v-for="instruction in data.instructions"
          class="mb-3 text-sm pr-24 text-justify text-slate-600"
        >
          {{ instruction }}
        </li>
      </ol>
    </div>
    <div>
      <img :src="data.gifUrl" alt="" />
    </div>
  </div>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({
  details: Object,
});

const data = props.details;
</script>
