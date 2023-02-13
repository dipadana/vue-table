<script setup lang="ts">
defineProps<{
  headers: { properties: string; label: string }[];
  datas: object[];
  footers?: any[];
}>();
</script>

<template>
  <table>
    <tr>
      <th v-for="(head, i) in headers" :key="i">{{ head.label }}</th>
      <slot name="extendHeaderTable"></slot>
    </tr>
    <tr v-for="(data, i) in datas" :key="i">
      <td v-for="(head, y) in headers" :key="y">
        {{
          head.properties !== ""
            ? data[head.properties as keyof unknown]
            : i + 1
        }}
      </td>
      <slot name="extendDataTable" :data="data"></slot>
    </tr>
    <tr>
      <td v-for="(footer, f) in footers" :key="f">
        {{ footer }}
      </td>
    </tr>
  </table>
</template>

<style scoped></style>
