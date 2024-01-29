<template>
  <div>
    <q-dialog v-model="popup">
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
          <q-input v-model="newExpense.title" label="Description" />
          <q-input prefix="$" v-model="newExpense.amount" label="Amount" />
          <q-btn
            text-color="white"
            @click="addNewExpense()"
            class="background2"
            label="Add Expense"
            :disabled="!formComplete"
          />
        </div>
      </div>
    </q-card>
    <q-card>
      <div v-if="monthlyExpenses.length" class="text-subtitle1">
        Monthly Expenses
      </div>
      <div class="row" v-for="expense in monthlyExpenses" :key="expense.title">
        <div class="text-body q-ma-sm">Expense: {{ expense.title }}</div>
        <div class="text-body q-ma-sm">Amount: {{ expense.amount }}</div>
      </div>
    </q-card>
  </div>
</template>

<script>
import { computed, ref } from "vue";
export default {
  name: "MonthlyExpenses",
  props: {
    isMobile: {
      type: Boolean,
      required: true,
    },
  },
  setup() {
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
  width: 300px;
  height: 220px;
}
.popup-card-dt {
  width: 30rem;
  height: 20rem;
}
</style>
