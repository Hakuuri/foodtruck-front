<template>
    <div class="profile-container">
      <h1>User Profile</h1>
      
      <!-- Show user information if available -->
      <div v-if="user">
        <p><strong>Username:</strong> {{ user.username }}</p>
        <p><strong>Email:</strong> {{ user.email }}</p>
        <p><strong>Phone:</strong> {{ user.phone }}</p>
        <p><strong>Date of Birth:</strong> {{ user.birthDate }}</p>
      </div>
  
      <!-- Show error message if there was an issue -->
      <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
  
      <!-- Show loader while fetching user data -->
      <p v-if="isLoading">Loading profile...</p>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  import { useRouter } from 'vue-router';
  
  const user = ref(null);        // Stores user data
  const isLoading = ref(true);    // Tracks loading state
  const errorMessage = ref('');   // Stores error message if fetching fails
  const router = useRouter();     // Vue Router instance for navigation
  
  // Fetch user profile data from the backend
  const fetchUserProfile = async () => {
    try {
      const response = await fetch(`http://localhost:8081/account?${document.cookie}`, {
        method: 'GET',
        credentials: 'include', // Ensure credentials (cookies) are sent with the request
      });

      if (response.ok) {
        const data = await response.json();
        console.log(data);  
        user.value = data; // Set user data
      } else {
        const error = await response.json();
        errorMessage.value = error.message || 'Failed to fetch profile';
      }
    } catch (error) {
      errorMessage.value = 'An error occurred while fetching profile';
    } finally {
      isLoading.value = false; // Hide the loading state
    }
  };
  
  // Fetch the profile data when the component is mounted
  onMounted(() => {
    fetchUserProfile();
  });
  </script>
  
  <style scoped>
  .profile-container {
    max-width: 600px;
    margin: 2em auto;
    padding: 2em;
    border: 1px solid #ccc;
    border-radius: 8px;
    background-color: #f9f9f9;
  }
  
  h1 {
    text-align: center;
    margin-bottom: 1em;
  }
  
  p {
    font-size: 1.2em;
    margin-bottom: 0.5em;
  }
  
  .error {
    color: red;
    text-align: center;
    margin-top: 1em;
  }
  
  .loading {
    text-align: center;
    font-size: 1.2em;
    color: gray;
  }
  </style>
  