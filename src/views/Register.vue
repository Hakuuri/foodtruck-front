<template>
    <div class="register-container">
      <h1>Inscription étudiant</h1>
      <form @submit.prevent="submitForm">
        <!-- Username -->
        <div class="form-group">
          <label for="username">Pseudo</label>
          <input
            type="text"
            id="username"
            v-model="username"
            required
            placeholder="Entre ton pseudo"
          />
        </div>
  
        <!-- Email -->
        <div class="form-group">
          <label for="email">Email</label>
          <input
            type="text"
            id="email"
            v-model="email"
            required
            placeholder="exemple@ecoles-epsi/wiz.net"
          />
        </div>
  
        <!-- Phone Number -->
        <div class="form-group">
          <label for="phone">N° de téléphone</label>
          <input
            type="number"
            id="phone"
            v-model="phone"
            required
            placeholder="N° de téléphone"
          />
        </div>
  
        <!-- Birth Date -->
        <div class="form-group">
          <label for="birth-date">Date de naissance</label>
          <input
            type="date"
            id="birth-date"
            v-model="birthDate"
            required
          />
        </div>
  
        <!-- Password -->
        <div class="form-group">
          <label for="password">Mot de passe</label>
          <input
            type="password"
            id="password"
            v-model="password"
            required
            placeholder="Enter your password"
          />
        </div>
  
        <!-- Confirm Password -->
        <div class="form-group">
          <label for="confirm-password">Confirmer Mot de passe</label>
          <input
            type="password"
            id="confirm-password"
            v-model="confirmPassword"
            required
            placeholder="Confirm your password"
          />
        </div>
  
        <!-- Display validation error messages -->
        <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
  
        <!-- Submit button -->
        <button type="submit">S'inscrire</button>
      </form>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  // Define reactive form inputs
  const username = ref('');
  const email = ref('');
  const password = ref('');
  const confirmPassword = ref('');
  const phone = ref('');
  const birthDate = ref('');
  const errorMessage = ref('');

  // Function to check if the selected birth date is valid
const isValidBirthDate = (birthDate) => {
  const today = new Date();
  const selectedDate = new Date(birthDate);

  // Check if the birth date is today or in the future
  if (selectedDate >= today) {
    return false;
  }
  
  return true;
};

  //  check if the email domain is valid
const isValidEmailDomain = (email) => {
  const validDomains = ["@ecoles-epsi.net", "@ecoles-wiz.net"];
  return validDomains.some((domain) => email.endsWith(domain));
};  
 // handle form submission
const submitForm = async () => {
  // Check if email domain is valid
  if (!isValidEmailDomain(email.value)) {
    errorMessage.value = "L'email doit être '@ecoles-epsi.net' ou '@ecoles-wiz.net'";
  } 
  // Check if passwords match
  else if (password.value !== confirmPassword.value) {
    errorMessage.value = "Les mots de passes ne correspondent pas";
  } else if (!isValidBirthDate(birthDate.value)) {
    errorMessage.value = "La date de naissance ne peut pas être aujourd'hui ni dans le futur";
  } else {
    errorMessage.value = '';
    // API call or registration logic
    console.log({
      username: username.value,
      email: email.value,
      phone: phone.value,
      birthDate: birthDate.value,
      password: password.value,
    });
    
    await fetch("http://localhost:8081/register",
{
    headers: {
      'Accept': 'application/json',
      'Content-Type': 'application/json'
    },
    method: "POST",
    body: JSON.stringify({username: username.value,
      email: email.value,
      phone: phone.value,
      birthDate: birthDate.value,
      password: password.value})
})
.then(function(res){ alert('Inscription réussie!'); })
.catch(function(res){ alert(' erreur Inscription ');  })
  }
};
  </script>
  
  <style scoped>
  .register-container {
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
  