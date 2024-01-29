<template>
  <q-page class="q-pa-md">
    <div class="row justify-center">
      <div class="text-h6">Simple Budget App</div>
      <q-btn v-if="budgeting" flat icon="home" @click="budgeting = false" />
      <q-space />
      <div class="col-3">
        <q-toggle
          v-model="toggle"
          color="green"
          :label="!toggle ? 'Custom Date Range' : 'Default Date Range'"
          :false-value="true"
          :true-value="false"
        />
      </div>
    </div>

    <div v-if="budgeting">
      <q-stepper flat ref="stepper" v-model="step">
        <q-step
          icon="assignment"
          title="Add Monthly Expenses"
          :name="1"
          :done="step > 1"
        >
          <monthly-expenses v-if="toggle" :isMobile="isMobile" />
          <custom-time-period v-else :isMobile="isMobile" />
        </q-step>
        <q-step title="Add Weekly Expenses" :name="2" :done="step > 2">
          <weekly-expenses v-if="toggle" :isMobile="isMobile" />
        </q-step>
        <q-step title="Add Income" :name="3" :done="step > 3">
          <income-predictor v-if="toggle" :isMobile="isMobile" />
        </q-step>
      </q-stepper>
      <q-separator />
      <div class="row justify-center">
        <q-btn
          v-if="step > 1"
          no-caps
          label="Back"
          @click="$refs.stepper.previous()"
        />
        <q-btn v-else @click="budgeting = false" label="Cancel" no-caps />
        <q-btn
          v-if="step < 3"
          no-caps
          label="Continue"
          @click="$refs.stepper.next()"
        />
        <q-btn v-else no-caps label="Finish" />
      </div>
    </div>
    <welcome-component v-else @start-budgeting="startBudgeting" />
  </q-page>
</template>

<script>
import { computed, ref } from "vue";
import { useQuasar } from "quasar";

import CustomTimePeriod from "src/components/CustomTimePeriod.vue";
import WelcomeComponent from "../components/WelcomeComponent.vue";
import MonthlyExpenses from "../components/MonthlyExpenses.vue";
import WeeklyExpenses from "../components/WeeklyExpenses.vue";

import IncomePredictor from "../components/IncomePredictor.vue";

export default {
  name: "HomePage",
  components: {
    WeeklyExpenses,
    MonthlyExpenses,
    WelcomeComponent,
    CustomTimePeriod,
    IncomePredictor,
  },
  setup() {
    const $q = useQuasar();
    const isMobile = computed(() => {
      return $q.screen.lt.md;
    });
    const budgeting = ref(false);
    const startBudgeting = () => {
      budgeting.value = true;
    };
    const toggle = ref(true);

    // Stepper Variables
    const step = ref(1);

    return {
      startBudgeting,
      budgeting,
      toggle,
      isMobile,
      step,
    };
  },
};
</script>
