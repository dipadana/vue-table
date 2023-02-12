<script setup lang="ts">
import { ref } from "vue";

interface Data {
  company: string;
  contact: string;
  country: string;
}

const headers = ref<{ properties: string; display: string }[]>([
  {
    properties: "",
    display: "No",
  },
  {
    properties: "company",
    display: "Company",
  },
  {
    properties: "contact",
    display: "Contact",
  },
  {
    properties: "country",
    display: "Country",
  },
]);

const datas = ref<Data[]>([
  {
    company: "Salim Group 1",
    contact: "Agus Salim Satu",
    country: "Belanda",
  },
  {
    company: "Salim Group 2",
    contact: "Agus Salim Dua",
    country: "Indonesia",
  },
  {
    company: "Salim Group 3",
    contact: "Agus Salim Tiga",
    country: "Singapura",
  },
  {
    company: "Salim Group 4",
    contact: "Agus Salim Empat",
    country: "USA",
  },
]);
</script>

<template>
  <p>Table</p>
  <table>
    <tr>
      <th v-for="(head, i) in headers" :key="i">{{ head.display }}</th>
    </tr>
    <tr v-for="(data, i) in datas" :key="i">
      <td v-for="(head, y) in headers" :key="y">
        {{
          head.properties !== "" ? data[head.properties as keyof Data] : i + 1
        }}
      </td>
    </tr>
  </table>
</template>

<style scoped></style>
