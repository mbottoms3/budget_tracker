<template>
  <div style="margin-top: 18rem">
    <q-card class="background-light-green" flat>
      <div class="row justify-center q-my-md">
        <div class="text-h3 text-white">Welcome to Simple Budget</div>
      </div>
      <div class="row justify-center">
        <div class="text-subtitle1 bold text-white">
          Click below to start budgeting better
        </div>
      </div>
      <div class="row justify-center q-mt-xl">
        <q-btn
          text-color="white"
          class="background-dark-green button-large"
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
