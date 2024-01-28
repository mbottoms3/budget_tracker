<template>
  <div>
    <q-card flat>
      <div class="text-subtitle1">Custom Time Period</div>
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
export default {
  name: "OnceAMonth",
  props: {
    isMobile: {
      type: Boolean,
      required: true,
    },
  },
  setup() {},
};
</script>
