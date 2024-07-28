<template>
  <div
    class="md:flex lg:flex justify-between bg-slate-100 mb-5 p-10 rounded-lg border-4 border-transparent hover:border-slate-900 focus:border-slate-500 cursor-pointer transition duration-300 ease-in-out opacity-85"
    v-for="data in props.details"
  >
    <div>
      <h1 class="text-xl mb-5 capitalize font-semibold">{{ data.name }}</h1>
      <ol>
        <li
          v-for="(instruction, index) in data.instructions"
          class="mb-3 text-sm lg:pr-24 md:pr-24 text-justify text-slate-600"
        >
          {{ index + 1 }}. {{ instruction }}
        </li>
      </ol>
    </div>
    <div>
      <img :src="data.gifUrl" alt="" class="rounded-lg shadow-sm opacity-100" />
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
