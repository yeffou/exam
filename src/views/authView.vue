<template>
  <div class="welcome">
    <div class="container">
      <div class="form-container">
        <h2>{{ showLogin ? 'Login' : 'Sign up' }}</h2>
        <div v-if="showLogin">
          <LoginForm @login="enterForum" />
          <p>No account yet? <span @click="showLogin = false">Sign up</span> instead.</p>
        </div>
        <div v-else>
          <SignupForm @signup="enterForum" />
          <p>Already registered? <span @click="showLogin = true">Login</span> instead.</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SignupForm from '../components/SignupForm.vue';
import LoginForm from '../components/LoginForm.vue';

export default {
  components: { SignupForm, LoginForm },
  data() {
    return {
      showLogin: true
    }
  },
  methods: {
    enterForum() {
      this.$router.push('/homeview');
    }
  }
}
</script>

<style scoped>
.welcome {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-image: linear-gradient(to bottom right, #F06666, #000000);
}

.container {
  max-width: 600px;
  width: 100%;
}

.form-container {
  background-color: rgba(255, 255, 255, 0.9);
  padding: 40px;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
}

h2 {
  font-size: 2rem;
  margin-bottom: 20px;
  color: #333;
}

p {
  color: #666;
  margin-top: 20px;
}

span {
  font-weight: bold;
  text-decoration: underline;
  cursor: pointer;
  color: #FF8080;
}

span:hover {
  color: #FF3333;
}
</style>
