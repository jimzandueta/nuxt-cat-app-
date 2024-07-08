<template>
  <v-container>
    <CatTable :cats="cats" :headers="headers" />
  </v-container>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { useFetch } from "#app";
import CatTable from "~/components/CatTable.vue";

interface Weight {
  imperial: string;
  metric: string;
}

interface Cat {
  id: string;
  name: string;
  origin: string;
  description: string;
  weight: Weight;
  life_span: string;
  temperament: string;
}

const cats = ref<Cat[]>([]);
const { data } = await useFetch<Cat[]>("https://api.thecatapi.com/v1/breeds");
if (data.value) {
  cats.value = data.value;
}

const headers = ref([
  { title: "Name", value: "name", sortable: true },
  { title: "Origin", value: "origin", sortable: true },
  { title: "Description", value: "description", sortable: false },
  { title: "Weight", value: "weight.metric", sortable: true },
  { title: "Lifespan", value: "life_span", sortable: true },
  { title: "Temperament", value: "temperament", sortable: false },
]);
</script>
