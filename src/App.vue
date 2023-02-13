<script setup lang="ts">
import { ref } from "vue";
import Table from "./Table.vue";

interface Data {
  company: string;
  credit: number;
  margin: number;
}

const headers = ref<{ properties: string; label: string }[]>([
  {
    properties: "",
    label: "No",
  },
  {
    properties: "company",
    label: "Company",
  },
  {
    properties: "credit",
    label: "Credit",
  },
  {
    properties: "margin",
    label: "Margin",
  },
]);

const datas = ref<Data[]>([
  {
    company: "Salim Group 1",
    credit: 5000,
    margin: 4000,
  },
  {
    company: "Salim Group 2",
    credit: 5000,
    margin: 4000,
  },
  {
    company: "Salim Group 3",
    credit: 5000,
    margin: 4000,
  },
  {
    company: "Salim Group 4",
    credit: 5000,
    margin: 4000,
  },
]);

const footers = ref<any[]>(["", "Total", 20000, 16000]);
</script>

<template>
  <Table :headers="headers" :datas="datas" :footers="footers">
    <template #extendHeaderTable><th>Action</th></template>
    <template #extendDataTable="tableProps">
      <td>Edit | Delete {{ tableProps.data }}</td>
    </template>
  </Table>
</template>

<style scoped></style>
