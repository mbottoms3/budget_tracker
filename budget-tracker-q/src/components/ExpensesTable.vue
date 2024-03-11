<template>
  <q-table
    dense
    flat
    bordered
    :rows="expenses"
    row-key="title"
    :columns="columns"
    separator="cell"
    hide-bottom
    :rows-per-page-options="[0]"
  >
  </q-table>
</template>
<script>
import { ref } from "vue";

export default {
  name: "ExpensesTable",
  props: {
    expenses: {
      type: Array,
      required: true,
    },
    isMobile: {
      type: Boolean,
      required: true,
    },
  },
  setup() {
    const columns = ref([
      {
        name: "title",
        label: "Expense",
        field: "title",
        align: "left",
        sortable: false,
      },
      {
        name: "amount",
        label: "Monthly Amount",
        field: "amount",
        align: "left",
        sortable: false,
      },
      {
        name: "frequency",
        label: "Frequency",
        field: "frequency",
        align: "left",
        sortable: false,
      },
      {
        name: "date",
        label: "Day of the Month",
        field: "date",
        align: "left",
        sortable: true,
      },
    ]);

    return {
      columns,
    };
  },
};
</script>
