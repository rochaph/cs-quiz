<template>
  <div>
    <b-container fluid align-self="center">
      <img class="logo" src="../assets/logo.png" alt="logo" />

      <h1>CS Quiz</h1>
    </b-container>
    <b-nav class="justify-content-center" tabs>
      <b-nav-item disabled>
        <strong>CS Quiz App</strong>
      </b-nav-item>
      <b-nav-item disabled>
        <strong>Counter: {{numCorrect}}/{{total}}</strong>
      </b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: {
    total: Number,
    numCorrect: Number
  }
};
</script>

<style scoped>
.logo {
  height: 120px;
  width: auto;
}
</style>