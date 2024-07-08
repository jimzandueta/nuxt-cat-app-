<template>
  <v-card flat>
    <v-card-title class="d-flex align-center pe-2">
      <v-icon icon="mdi-cat"></v-icon> &nbsp; Find a Cat Breed

      <v-spacer></v-spacer>

      <v-text-field
        v-model="search"
        density="compact"
        label="Search"
        prepend-inner-icon="mdi-magnify"
        variant="solo-filled"
        flat
        hide-details
        single-line
      ></v-text-field>
    </v-card-title>

    <v-divider></v-divider>

    <v-data-table
      v-model="selected"
      :search="search"
      :headers="headers"
      :items="cats"
      item-value="id"
      items-per-page="10"
      return-object
      show-select
      class="elevation-2"
    >
      <template v-slot:[`item.name`]="{ item }">
        {{ item.name }}
      </template>
      <template v-slot:[`item.origin`]="{ item }">
        {{ item.origin }}
      </template>
      <template v-slot:[`item.description`]="{ item }">
        {{ item.description }}
      </template>
      <template v-slot:[`item.weight`]="{ item }">
        {{ item.weight.metric }} kg
      </template>
      <template v-slot:[`item.lifespan`]="{ item }">
        {{ item.life_span }} years
      </template>
      <template v-slot:[`item.temperament`]="{ item }">
        {{ item.temperament }}
      </template>
    </v-data-table>

    <v-card-text>
      <pre>{{ selected }}</pre>
    </v-card-text>
  </v-card>
</template>

<script setup lang="ts">
import { ref, defineProps, watch } from "vue";

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

const props = defineProps<{
  cats: Cat[];
  headers: { title: string; value: string; sortable: boolean }[];
}>();

const search = ref("");
const selected = ref<Cat[]>([]);

watch(selected, () => {
  const duplicates = selected.value.filter(
    (item, index) => selected.value.indexOf(item) !== index
  );
  if (duplicates.length > 0) {
    selected.value = selected.value.filter((item) => item !== duplicates[0]);
  }
});
</script>
