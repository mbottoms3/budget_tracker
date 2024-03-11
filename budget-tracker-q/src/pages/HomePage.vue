<template>
  <q-page :class="budgeting ? '' : 'bg-primary'" class="q-pa-md">
    <div class="row justify-center">
      <div class="text-h6">Simple Budget App</div>
      <q-space />
      <q-btn
        size="md"
        v-if="budgeting"
        flat
        icon="home"
        @click="budgeting = false"
      />

      <!-- <div class="col-3">
        <q-toggle
          v-model="toggle"
          color="green"
          :label="!toggle ? 'Custom Date Range' : 'Default Date Range'"
          :false-value="true"
          :true-value="false"
        />
      </div> -->
    </div>

    <div v-if="budgeting">
      <q-stepper
        alternative-labels
        :contracted="isMobile"
        flat
        ref="stepper"
        v-model="step"
      >
        <q-step
          icon="assignment"
          title="Add Monthly Expenses"
          :name="1"
          :done="step > 1"
        >
          <monthly-expenses
            @monthly="handleMonthlyExpenses"
            v-if="toggle"
            :isMobile="isMobile"
          />
          <custom-time-period v-else :isMobile="isMobile" />
        </q-step>
        <q-step title="Add Weekly Expenses" :name="2" :done="step > 2">
          <weekly-expenses
            @weekly="handleWeeklyExpenses"
            v-if="toggle"
            :isMobile="isMobile"
          />
        </q-step>
        <q-step title="Review Expenses" :name="3" :done="step > 3">
          <expenses-table :isMobile="isMobile" :expenses="allExpenses" />
        </q-step>
        <q-step title="Add Income" :name="4" :done="step > 4">
          <income-predictor
            @income="handleIncome"
            v-if="toggle"
            :isMobile="isMobile"
          />
        </q-step>
      </q-stepper>
      <q-separator />
      <div class="row justify-center q-mt-xl">
        <q-btn
          :class="isMobile ? 'button-med' : 'button-large'"
          class="bg-dark text-white q-mx-md"
          v-if="step > 1"
          no-caps
          label="Back"
          @click="$refs.stepper.previous()"
        />
        <q-btn
          :class="isMobile ? 'button-med' : 'button-large'"
          class="bg-dark text-white q-mx-md"
          v-else
          @click="budgeting = false"
          label="Cancel"
          no-caps
        />
        <q-btn
          :class="isMobile ? 'button-med' : 'button-large'"
          class="bg-accent q-mx-md"
          v-if="step < 4"
          no-caps
          label="Continue"
          @click="$refs.stepper.next()"
        />

        <q-btn v-else no-caps label="Finish" />
      </div>
    </div>
    <welcome-component v-else @start-budgeting="startBudgeting" />
    <!-- <div>{{ weeklyExpenses }}</div>
    <div>{{ monthlyExpenses }}</div>
    <div>{{ income }}</div> -->
  </q-page>
</template>

<script>
import { computed, ref, watch } from "vue";
import { useQuasar } from "quasar";

import CustomTimePeriod from "src/components/CustomTimePeriod.vue";
import WelcomeComponent from "../components/WelcomeComponent.vue";
import MonthlyExpenses from "../components/MonthlyExpenses.vue";
import WeeklyExpenses from "../components/WeeklyExpenses.vue";
import ExpensesTable from "src/components/ExpensesTable.vue";
import IncomePredictor from "../components/IncomePredictor.vue";

export default {
  name: "HomePage",
  components: {
    WeeklyExpenses,
    MonthlyExpenses,
    WelcomeComponent,
    CustomTimePeriod,
    IncomePredictor,
    ExpensesTable,
  },
  setup() {
    const $q = useQuasar();
    const isMobile = computed(() => {
      return $q.screen.lt.md;
    });
    const budgeting = ref(false);
    const startBudgeting = () => {
      step.value = 1;
      budgeting.value = true;
    };
    const toggle = ref(true);

    // Stepper Variables
    const step = ref(1);

    // Monthly Expenses
    const monthlyExpenses = ref([]);
    const handleMonthlyExpenses = (newVal) => {
      monthlyExpenses.value = newVal;
    };
    // Weekly Expenses
    const weeklyExpenses = ref([]);
    const handleWeeklyExpenses = (newVal) => {
      weeklyExpenses.value = newVal;
      console.log(weeklyExpenses.value);
    };
    // Income
    const income = ref({
      amount: "",
      frequency: "",
    });
    const handleIncome = (newVal) => {
      console.log(newVal);
      income.value = newVal;
    };

    // Prepare expenses for table
    const allExpenses = ref([]);
    const expenseFormatter = () => {
      // Process Monthly Expenses
      monthlyExpenses.value.forEach((expense) => {
        const formattedExpense = {
          title: expense.title,
          amount: expense.amount,
          frequency: "monthly",
          date: expense.date,
        };
        allExpenses.value.push(formattedExpense);
      });

      // Process Weekly Expenses
      weeklyExpenses.value.forEach((expense) => {
        const formattedExpense = {
          title: expense.title,
          amount: expense.amount * 4, // Multiply weekly amount by 4
          frequency: "weekly",
          date: expense.date,
        };
        allExpenses.value.push(formattedExpense);
      });
    };
    watch(step, () => {
      console.log(step.value);
      if (step.value == 3) {
        expenseFormatter();
      }
    });

    return {
      startBudgeting,
      budgeting,
      toggle,
      isMobile,
      step,
      monthlyExpenses,
      weeklyExpenses,
      income,
      handleMonthlyExpenses,
      handleWeeklyExpenses,
      handleIncome,
      expenseFormatter,
      allExpenses,
    };
  },
};
</script>
