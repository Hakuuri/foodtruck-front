<script setup>
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';

const isMobileMenuOpen = ref(false);
const isLoggedIn = ref(false); 
const router = useRouter();

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value;
};

// Check if the user is logged in 
const checkLoginStatus = async () => {
  try {
    const response = await fetch('http://localhost:8081/profile', {
      method: 'GET',
      headers: {
          'Content-Type': 'application/json'  
        },
      credentials: 'include', 
    });

    console.log(response)
    if (response.ok) {
      isLoggedIn.value = true;
    } else {
      isLoggedIn.value = false;
    }
  } catch (error) {
    isLoggedIn.value = false;
  }
};

// Handle user logout
const handleLogout = async () => {
  try {
    const response = await fetch('http://localhost:8081/logout', {
      method: 'POST',
      credentials: 'include', // include session cookies
    });
    if (response.ok) {
      isLoggedIn.value = false;
      router.push('/login'); // Redirect to login page
    }
  } catch (error) {
    console.error('Error during logout:', error);
  }
};

// On component mount, check login status
onMounted(() => {
  checkLoginStatus();
});
</script>

<template>
  <header>
    <nav class="navbar">
      <div class="navbar-brand">
        <RouterLink to="/" class="nav-btn">Home</RouterLink>
      </div>

      <!-- Hamburger menu icon for mobile -->
      <button class="hamburger" @click="toggleMobileMenu">
        <span class="hamburger-bar"></span>
        <span class="hamburger-bar"></span>
        <span class="hamburger-bar"></span>
      </button>

      <!-- Links on desktop view and for expanded mobile menu -->
      <div :class="['navbar-links', { 'open': isMobileMenuOpen }]">
        <!-- Show "Register" only if the user is NOT logged in -->
        <RouterLink v-if="!isLoggedIn" to="/register" class="nav-btn">Register</RouterLink>
        <RouterLink v-if="!isLoggedIn" to="/login" class="nav-btn">Login</RouterLink>

        <!-- Show "Logout" button if the user IS logged in -->
        <button v-if="isLoggedIn" @click="handleLogout" class="nav-btn">Logout</button>
      </div>
    </nav>
  </header>

  <main>
    <RouterView />
  </main>
</template>

<style scoped>
html {
  margin: 0;
}

body {
  margin: 0;
}

.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #42b983;
  padding: 1em;
  position: relative;
}

.navbar-brand {
  font-size: 1.5em;
}

.nav-btn {
  text-decoration: none;
  color: white;
  padding: 0.75em 1.25em;
  background-color: #42b983;
  border: 2px solid white;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}

.nav-btn:hover {
  background-color: white;
  color: #42b983;
}

.hamburger {
  display: none;
  flex-direction: column;
  justify-content: space-between;
  width: 30px;
  height: 24px;
  background: none;
  border: none;
  cursor: pointer;
}

.hamburger-bar {
  height: 3px;
  width: 100%;
  background-color: white;
  transition: 0.3s;
}

/* Links container for desktop and mobile */
.navbar-links {
  display: flex;
}

.navbar-links a { 
  margin-left: 20px;
}

/* Hamburger menu for mobile */
@media (max-width: 768px) {
  .navbar-links {
    display: none;
    flex-direction: column;
    position: absolute;
    top: 100%;
    right: 0;
    background-color: #42b983;
    width: 100%;
  }

  .navbar-links.open {
    display: flex;
  }

  .hamburger {
    display: flex;
  }

  .nav-btn {
    width: 100%;
    text-align: center;
    border-top: 1px solid white;
  }

  .nav-btn:first-of-type {
    border-top: none;
  }
}
</style>
