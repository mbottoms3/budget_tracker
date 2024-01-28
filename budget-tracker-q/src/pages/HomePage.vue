<template>
  <q-page class="q-pa-md">
    <div class="row justify-center">
      <div class="text-h6">Budget Tracker App</div>
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
    <once-a-month v-if="toggle" :isMobile="isMobile" />
    <custom-time-period v-else :isMobile="isMobile" />
  </q-page>
</template>

<script>
import { computed, ref } from "vue";
import { useQuasar } from "quasar";
import OnceAMonth from "src/components/OnceAMonth.vue";
import CustomTimePeriod from "src/components/CustomTimePeriod.vue";

export default {
  name: "HomePage",
  components: {
    OnceAMonth,

    CustomTimePeriod,
  },
  setup() {
    const $q = useQuasar();
    const isMobile = computed(() => {
      return $q.screen.lt.md;
    });

    const toggle = ref(true);

    return {
      toggle,
      isMobile,
    };
  },
};
</script>
