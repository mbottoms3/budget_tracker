<template>
  <div style="margin-top: 18rem">
    <q-card class="bg-primary" flat>
      <div class="row justify-center q-my-md">
        <div class="text-h3 text-white text-center">
          Welcome to Simple Budget
        </div>
      </div>

      <div class="text-subtitle1 bold text-white text-center">
        Click below to start budgeting better
      </div>

      <div class="row justify-center q-mt-xl">
        <q-btn
          text-color="white"
          class="bg-secondary button-large"
          @click="$emit('startBudgeting')"
          label="Get Started"
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
