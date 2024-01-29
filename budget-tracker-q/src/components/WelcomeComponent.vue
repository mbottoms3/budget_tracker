<template>
  <div>
    <q-card flat>
      <div class="row justify-center">
        <div class="text-h6">Welcome to Simple Budget App!</div>
      </div>
      <div class="row justify-center">
        <q-btn
          @click="$emit('startBudgeting')"
          label="Begin"
          no-caps
          size="lg"
        />
      </div>
    </q-card>
  </div>
</template>
<script>
export default {
  name: "WelcomeComponent",
};
</script>

<style></style>
