<template>
    <div class="login-container">
      <h1>Login</h1>
      <form @submit.prevent="submitLogin">
        <div class="form-group">
          <label for="email">Email</label>
          <input type="text" id="email" v-model="email" required placeholder="Enter your email" />
        </div>
  
        <div class="form-group">
          <label for="password">Password</label>
          <input type="password" id="password" v-model="password" required placeholder="Enter your password" />
        </div>
  
        <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
  
        <button type="submit">Login</button>
      </form>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import { useRouter } from 'vue-router';
  
  const email = ref('');
  const password = ref('');
  const errorMessage = ref('');
  const router = useRouter();
  
  const submitLogin = async () => {
    try {
      const response = await fetch('http://localhost:8081/login', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ email: email.value, password: password.value }),
      });
  
      if (response.ok) {
        const data = await response.json();
        alert('Login successful!');
        document.cookie = `userEmail=${data.user.email}; path=/; max-age=86400`;
        console.log('Logged in user:', data.user);
        // Redirect to the home page
        router.push('/');
      } else {
        const error = await response.json();
        errorMessage.value = error.message;
      }
    } catch (error) {
      errorMessage.value = 'An error occurred during login';
    }
  };
  </script>
  
  <style scoped>
  .login-container {
    max-width: 400px;
    margin: 0 auto;
    padding: 2em;
    border: 1px solid #ccc;
    border-radius: 8px;
  }
  
  h1 {
    text-align: center;
    margin-bottom: 1em;
  }
  
  .form-group {
    margin-bottom: 1em;
  }
  
  label {
    display: block;
    margin-bottom: 0.5em;
    font-weight: bold;
  }
  
  input {
    width: 100%;
    padding: 0.5em;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  
  button {
    width: 100%;
    padding: 0.75em;
    background-color: #42b983;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 1em;
  }
  
  button:hover {
    background-color: #369a75;
  }
  
  .error {
    color: red;
    margin-bottom: 1em;
    text-align: center;
  }
  </style>
  