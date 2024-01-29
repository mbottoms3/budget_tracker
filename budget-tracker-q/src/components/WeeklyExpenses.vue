<template>
  <div>
    <q-dialog v-model="popup">
      <q-card class="background">
        <div class="text-subtitle1">
          Let's start off by adding consistent, monthly expenses.
        </div>
        <div class="text-subtitle2">
          Don't worry, you'll be able to add weekly/infrequent expenses too!
        </div>
        <!-- Gif showing expenses being added -->
        <div class="row">
          <q-btn @click="popup = false" label="Ready!" no-caps />
        </div>
      </q-card>
    </q-dialog>
    <q-card flat>
      <div class="text-subtitle1">Weekly Expenses</div>
      <div class="row">
        <div class="col-xs-12 col-md-6">
          <div class="text-subtitle1">Enter a Recurring Weekly Expense</div>
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
      <div class="row" v-for="expense in weeklyExpenses" :key="expense.title">
        {{ expense.title }} + {{ expense.amount }}
      </div>
    </q-card>
  </div>
</template>

<script>
import { computed, ref } from "vue";
export default {
  name: "WeeklyExpenses",
  props: {
    isMobile: {
      type: Boolean,
      required: true,
    },
  },
  setup() {
    const popup = ref(true);
    const weeklyExpenses = ref([]);
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
      weeklyExpenses.value.push(newExpense.value);
    };

    return {
      popup,
      weeklyExpenses,
      addNewExpense,
      newExpense,
      formComplete,
    };
  },
};
</script>

<style scoped>
.background {
  background: #34823e;
}
</style>
