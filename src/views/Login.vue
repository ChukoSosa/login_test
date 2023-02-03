<template>
  <div class="login">
    <LoginForm />
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import LoginForm from './../components/LoginForm.vue'; // @ is an alias to /src

export default Vue.extend({
  name: 'Login',
  components: {
    LoginForm,
  },
});
</script>
