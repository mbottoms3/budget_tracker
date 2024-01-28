<template>
  <div>
    <q-card flat>
      <div class="text-subtitle1">Monthly Expenses</div>
      <div class="row">
        <div class="col-xs-12 col-md-6">
          <div class="text-subtitle1">Enter a Recurring Monthly Expense</div>
          <q-input v-model="newExpense.title" label="Title" />
          <q-input prefix="$" v-model="newExpense.amount" label="Amount" />
          <q-btn
            @click="addNewExpense()"
            color="primary"
            label="Add Expense"
            :disabled="!formComplete"
          />
        </div>
      </div>
    </q-card>
    <q-card>
      <div class="row" v-for="expense in monthlyExpenses" :key="expense.title">
        {{ expense.title }} + {{ expense.amount }}
      </div>
    </q-card>
    <q-separator />
    <q-card flat>
      <div class="text-subtitle1">Weekly Expenses</div>
    </q-card>
    <q-separator />
    <q-card flat>
      <div class="text-subtitle1">Weekly Expenses</div>
    </q-card>
  </div>
</template>

<script>
import { computed, ref } from "vue";
export default {
  name: "OnceAMonth",
  props: {
    isMobile: {
      type: Boolean,
      required: true,
    },
  },
  setup() {
    const monthlyExpenses = ref([]);
    const newExpense = ref({
      title: "",
      amount: "",
    });
    const formComplete = computed(() => {
      return (
        newExpense.value.title.trim() !== "" &&
        newExpense.value.amount.trim() !== ""
      );
    });
    const addNewExpense = () => {
      monthlyExpenses.value.push(newExpense.value);
    };

    return {
      monthlyExpenses,
      addNewExpense,
      newExpense,
      formComplete,
    };
  },
};
</script>
