<template>
  <div class="table">
    <div class="table__container">
      <div class="table__header">
        <p>{{ tableTitle }}</p>

        <!-- Place for button beside title -->
        <slot name="headerButton"></slot>
      </div>
      <div class="table__box">
        <table rules="none">
          <!-- Table Head -->
          <tr class="table__head">
            <th v-for="(data, i) in headData" :key="i">{{ data.label }}</th>

            <!-- Slot for extend number of table heads, usually for action -->
            <slot name="extendHeaderTable"></slot>
          </tr>

          <!-- Table Data -->
          <tr class="table__items" v-for="(data, j) in tableData" :key="j">
            <td v-for="(dataProperties, k) in headDataProperties" :key="k">
              <!-- Complex data table slot , where passing object data { data, head } -->
              <slot
                name="dataTableCustom"
                :dataTableProps="{
                  data: getObj(
                    data,
                    dataProperties.properties,
                    dataProperties.default ?? ''
                  ),
                  head: dataProperties.properties,
                }"
                >{{
                  getObj(
                    data,
                    dataProperties.properties,
                    dataProperties.default ?? ""
                  )
                }}</slot
              >
            </td>

            <!-- Slot for extend number of table line/data, usually for action -->
            <slot name="extendDataTable" :data="data"></slot>
          </tr>

          <!-- Slot for uniq footer data, like total, sum, etc -->
          <tr
            v-if="footerData.length"
            class="table__items table__items--footer-data"
          >
            <td v-for="(footerData, l) in footerData" :key="l">
              <slot name="footerSlot" :footerProps="footerData">{{
                footerData
              }}</slot>
            </td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";

export default defineComponent({
  props: {
    tableTitle: {
      type: String,
      default: "Table Title",
    },
    headData: {
      type: Array,
      required: true,
    },
    tableData: {
      type: Array,
      required: true,
    },
    footerData: {
      type: Array,
      default: [],
    },
  },
  setup(props) {
    const headDataProperties = ref<any>([]);
    props.headData.forEach((element: any) => {
      headDataProperties.value.push({
        properties: element.properties,
        default: element.default,
      });
    });

    // Function for get nested object value from string ("obj.subObj.subObjObj")
    function getObj(
      obj: Record<string, any>,
      path: string,
      defaultValue?: any
    ) {
      console.log(obj, path, defaultValue);
      const fullPath = path
        .replace(/\[/g, ".")
        .replace(/]/g, "")
        .split(".")
        .filter(Boolean);

      const isNotEmptyData = fullPath.every((step) => {
        if (!step) return false;
        obj = obj[step];
        return obj !== undefined && obj !== null;
      });
      if (isNotEmptyData) return obj;
      else return defaultValue;
    }

    return {
      headDataProperties,
      getObj,
    };
  },
});
</script>

<style lang="scss">
.table {
  align-items: center;
  display: flex;
  justify-content: center;
  margin: auto;
  min-width: 1088px;

  @media only screen and (min-width: 576px) {
    max-width: 100%;
    min-width: 0;
  }

  &__container {
    width: 100%;
  }

  &__header {
    align-items: center;
    background: rgba(#ffffff, 0.8);
    border-top-left-radius: 8px;
    border-top-right-radius: 8px;
    display: flex;
    padding: 26px 32px 22px;

    p {
      color: black;
      font-size: 18px;
      font-weight: 700;
      line-height: (22 / 18);
      margin: 0 30px 0 0;
    }
  }

  table {
    width: 100%;
  }

  th,
  td {
    border: none;
    text-align: left;
  }

  &__box {
    overflow-x: auto;
  }

  &__head {
    background-color: rgba(#f7f8fc, 0.8);
    padding: 0 32px;

    th {
      color: black;
      font-size: 14px;
      font-weight: 700;
      line-height: (17 / 14);
      padding: 16px 32px;
      text-transform: capitalize;
    }
  }

  &__items {
    background: rgba(#ffffff, 0.8);
    border-top: 1px solid rgba(#000000, 0.08);

    td {
      color: black;
      font-size: 14px;
      font-weight: 500;
      line-height: (17 / 14);
      padding: 16px 0 16px 32px;
    }

    &--footer-data {
      background: rgba(blue, 0.8);

      td {
        color: #ffffff;
        font-weight: 700;
      }
    }
  }

  tr:last-child td:last-child {
    border-bottom-right-radius: 8px;
  }

  tr:last-child td:first-child {
    border-bottom-left-radius: 8px;
  }

  tr:nth-child(2) {
    border-top: 1px solid rgba(#000000, 0) !important;
  }
}
</style>
