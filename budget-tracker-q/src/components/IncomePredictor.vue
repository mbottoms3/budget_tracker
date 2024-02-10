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
      <div class="text-subtitle1">Add in your average income</div>
      <div class="row">
        <div class="col-xs-12 col-md-6">
          <div class="text-subtitle1">Enter a Recurring Weekly Expense</div>

          <q-input prefix="$" v-model="newIncome.amount" label="Amount" />
          <q-select
            label="Income Frequency"
            v-model="newIncome.frequency"
            :options="options"
          />
          <q-btn
            @click="addIncome()"
            color="primary"
            label="Add Income"
            :disabled="!formComplete"
          />
        </div>
      </div>
    </q-card>
    <q-card> </q-card>
  </div>
</template>

<script>
import { computed, ref, watch } from "vue";
export default {
  name: "IncomePredictor",
  props: {
    isMobile: {
      type: Boolean,
      required: true,
    },
  },
  emits: ["income"],
  setup(props, context) {
    const popup = ref(true);
    const newIncome = ref({
      amount: "",
      frequency: "",
    });
    const monthlyTotal = ref({
      amount: "",
      frequency: "",
    });
    const options = ref([
      {
        label: "Once a month",
        multiplier: "1",
      },
      {
        label: "Twice a month",
        multiplier: "2",
      },
      {
        label: "Weekly",
        multiplier: "4",
      },
    ]);

    const addIncome = () => {
      console.log(newIncome.value);
      monthlyTotal.value = newIncome.value;
      console.log(monthlyTotal.value);
    };
    const formComplete = computed(() => {
      return newIncome.value.amount.trim() !== "";
    });

    watch(monthlyTotal, (newValue, oldValue) => {
      context.emit("income", newValue);
    });

    return {
      popup,
      options,
      addIncome,
      newIncome,
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
