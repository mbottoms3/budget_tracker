<template>
  <div>
    <q-dialog v-model="popup">
      <q-card
        :class="
          isMobile ? 'popup-card-mobile q-pa-sm' : 'popup-card-dt q-pa-md'
        "
        class="popup-card-style"
      >
        <div
          class="text-center q-my-md"
          :class="isMobile ? 'text-h5 text-bold' : 'text-h4'"
        >
          Let's start off by adding consistent, monthly expenses.
        </div>

        <div
          class="text-center"
          :class="isMobile ? 'text-subtitle2' : 'text-subtitle1'"
        >
          Don't worry, you'll be able to add weekly/infrequent expenses too!
        </div>
        <!-- Gif showing expenses being added -->
        <div class="row justify-center q-mt-xl">
          <q-btn
            class="button-large"
            text-color="white"
            color="secondary"
            @click="popup = false"
            label="Ready!"
            no-caps
          />
        </div>
      </q-card>
    </q-dialog>

    <!-- Input Card -->
    <div class="row justify-center">
      <div class="col-xs-12 col-md-7">
        <q-card flat class="bg-accent expense-input-card q-py-md q-px-sm">
          <div class="row justify-center">
            <div class="text-subtitle1">Monthly Expenses</div>
          </div>
          <div class="row justify-center">
            <div class="col-xs-12 col-md-4">
              <div class="text-subtitle1">
                Enter a Recurring Monthly Expense
              </div>
              <q-input
                class="q-my-sm"
                rounded
                outlined
                bg-color="white"
                dense
                v-model="newExpense.title"
                label="Description"
              />
              <q-input
                class="q-my-sm"
                rounded
                bg-color="white"
                outlined
                dense
                prefix="$"
                v-model="newExpense.amount"
                label="Amount"
              />
              <q-input
                rounded
                outlined
                bg-color="white"
                dense
                v-model="newExpense.date"
                disable
                class="input-style q-my-sm"
                label="Date"
              >
                <template v-slot:after>
                  <q-btn
                    color="primary"
                    @click="datePicker = true"
                    flat
                    icon="event"
                  />
                </template>
              </q-input>
              <q-dialog v-model="datePicker">
                <q-date mask="MM-DD-YYYY" today-btn v-model="newExpense.date" />
              </q-dialog>

              <q-btn
                class="button-med"
                no-caps
                text-color="positive"
                color="white"
                @click="addNewExpense()"
                label="Add Expense"
                :disabled="!formComplete"
              />
            </div>
          </div>
        </q-card>
      </div>
    </div>

    <!-- Output card -->
    <q-card flat class="q-mt-xl">
      <div v-if="monthlyExpenses.length" class="text-h6 text-center">
        Monthly Expenses
      </div>
      <div class="row justify-center">
        <div
          class="col-md-7 col-xs-12"
          v-for="expense in monthlyExpenses"
          :key="expense.title"
        >
          <q-card class="expense-output-card q-my-md">
            <div class="text-h6 text-center q-ma-sm">
              {{ expense.title }}
            </div>

            <div class="text-subtitle1 text-center q-ma-sm">
              ${{ expense.amount }} on {{ expense.date }}
            </div>
          </q-card>
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
    const datePicker = ref(false);
    const monthlyExpenses = ref([]);
    const newExpense = ref({
      title: "",
      amount: "",
      date: "",
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
        date: "",
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
      datePicker,
    };
  },
};
</script>

<style scoped>
.popup-card-style {
  border-radius: 16px;
  border: 3px solid #1e7fc9;
}
.popup-card-mobile {
  width: 350px;
  height: 300px;
}
.popup-card-dt {
  width: 500px;
  height: 400px;
}
</style>
