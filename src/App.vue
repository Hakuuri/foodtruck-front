<script setup>
import { ref } from 'vue';

const isMobileMenuOpen = ref(false);

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value;
};
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
        <RouterLink to="/register" class="nav-btn">Register</RouterLink>
      </div>
    </nav>
  </header>

  <main>
    <RouterView />
  </main>
</template>

<style scoped>
html{
  margin: 0;
}
body{
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
