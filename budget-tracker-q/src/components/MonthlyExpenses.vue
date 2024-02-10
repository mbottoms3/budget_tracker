<template>
  <div>
    <q-dialog :maximized="$q.screen.lt.sm ? true : false" v-model="popup">
      <q-card
        :class="isMobile ? 'popup-card-mobile' : 'popup-card-dt'"
        class="popup-card-style"
      >
        <div class="text-h4">
          Let's start off by adding consistent, monthly expenses.
        </div>
        <div class="text-subtitle1">
          Don't worry, you'll be able to add weekly/infrequent expenses too!
        </div>
        <!-- Gif showing expenses being added -->
        <div class="row">
          <q-btn
            text-color="white"
            class="background2"
            @click="popup = false"
            label="Ready!"
            no-caps
          />
        </div>
      </q-card>
    </q-dialog>
    <q-card flat>
      <div class="text-subtitle1">Monthly Expenses</div>
      <div class="row">
        <div class="col-xs-12 col-md-6">
          <div class="text-subtitle1">Enter a Recurring Monthly Expense</div>
          <q-input
            outlined
            bg-color="accent"
            dense
            v-model="newExpense.title"
            label="Description"
          />
          <q-input
            bg-color="accent"
            outlined
            dense
            prefix="$"
            v-model="newExpense.amount"
            label="Amount"
          />
          <q-btn
            color="secondary"
            @click="addNewExpense()"
            label="Add Expense"
            :disabled="!formComplete"
          />
        </div>
      </div>
    </q-card>
    <q-card flat>
      <div v-if="monthlyExpenses.length" class="text-subtitle1">
        Monthly Expenses
      </div>
      <div class="row">
        <div
          v-for="expense in monthlyExpenses"
          :key="expense.title"
          class="col-xs-12 col-md-6 bg-primary q-my-md"
        >
          <div class="text-body q-ma-sm">Expense: {{ expense.title }}</div>
          <div class="text-body q-ma-sm">Amount: {{ expense.amount }}</div>
        </div>
      </div>
    </q-card>
  </div>
</template>

<script>
import { computed, ref, watch } from "vue";
export default {
  name: "MonthlyExpenses",
  props: {
    isMobile: {
      type: Boolean,
      required: true,
    },
  },
  emits: ["monthly"],
  setup(props, context) {
    const popup = ref(true);
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

      resetForm();
    };

    const resetForm = () => {
      newExpense.value = {
        title: "",
        amount: "",
      };
    };

    watch(monthlyExpenses.value, () => {
      context.emit("monthly", monthlyExpenses.value);
    });

    return {
      popup,
      monthlyExpenses,
      addNewExpense,
      newExpense,
      formComplete,
    };
  },
};
</script>

<style scoped>
.popup-card-style {
  border: 4px solid #519e5b;
  border-radius: 8px;
}
.popup-card-mobile {
  width: 400px;
  height: 300px;
}
.popup-card-dt {
  width: 500px;
  height: 400px;
}
</style>
