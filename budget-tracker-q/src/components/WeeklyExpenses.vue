<template>
  <div>
    <!-- Input Card -->
    <div class="row justify-center">
      <div class="col-xs-12 col-md-7">
        <q-card flat class="bg-accent expense-input-card q-py-md q-px-sm">
          <div class="row justify-center">
            <div class="text-subtitle1">Weekly Expenses</div>
          </div>
          <div class="row justify-center">
            <div class="col-xs-12 col-md-4">
              <div class="text-subtitle1">Enter a Recurring Weekly Expense</div>
              <q-input
                class="q-my-sm"
                rounded
                outlined
                color="primary"
                label-color="positive"
                bg-color="white"
                dense
                v-model="newExpense.title"
                label="Description"
              />
              <q-input
                color="primary"
                label-color="positive"
                class="q-my-sm"
                rounded
                bg-color="white"
                outlined
                dense
                prefix="$"
                v-model="newExpense.amount"
                label="Amount"
              />

              <q-select
                label="Day of the week"
                dense
                outlined
                rounded
                bg-color="white"
                label-color="positive"
                color="primary"
                :options="weekdayOptions"
                v-model="newExpense.weekday"
                class="q-my-sm"
              />
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
      <div v-if="weeklyExpenses.length" class="text-h6 text-center">
        Monthly Expenses
      </div>
      <div class="row justify-center">
        <div
          class="col-md-7 col-xs-12"
          v-for="(expense, index) in weeklyExpenses"
          :key="expense.title"
        >
          <q-card class="expense-output-card q-my-md">
            <div class="row justify-center">
              <q-space />
              <div class="text-h6 text-center q-ma-sm">
                {{ expense.title }}
              </div>
              <q-space />
              <q-btn
                flat
                icon="close"
                padding="sm"
                size="sm"
                color="dark"
                @click="removeExpense(index)"
              />
            </div>

            <div class="text-subtitle1 text-center q-ma-sm">
              ${{ expense.amount }} each {{ expense.weekday.label }}
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
  name: "WeeklyExpenses",
  props: {
    isMobile: {
      type: Boolean,
      required: true,
    },
  },
  emits: ["weekly"],
  setup(props, context) {
    const weeklyExpenses = ref([]);
    const newExpense = ref({
      title: "",
      amount: "",
      weekday: "",
    });

    const weekdayOptions = ref([
      {
        label: "Sunday",
        value: "sunday",
      },
      {
        label: "Monday",
        value: "monday",
      },
      {
        label: "Tuesday",
        value: "tuesday",
      },
      {
        label: "Wednesday",
        value: "wednesday",
      },
      {
        label: "Thursday",
        value: "thursday",
      },
      {
        label: "Friday",
        value: "friday",
      },
      {
        label: "Saturday",
        value: "saturday",
      },
    ]);

    const formComplete = computed(() => {
      return (
        newExpense.value.title.trim() !== "" &&
        newExpense.value.amount.trim() !== ""
      );
    });
    const addNewExpense = () => {
      weeklyExpenses.value.push(newExpense.value);

      resetForm();
    };

    const removeExpense = (index) => {
      weeklyExpenses.value.splice(index, 1);
    };

    const resetForm = () => {
      newExpense.value = {
        title: "",
        amount: "",
        weekday: "",
      };
    };

    watch(weeklyExpenses.value, () => {
      context.emit("weekly", weeklyExpenses.value);
    });

    return {
      removeExpense,
      weekdayOptions,
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
