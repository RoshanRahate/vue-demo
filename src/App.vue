<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <nav>
      <div>
        <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="40px" height="40px" />
      </div>
      <div class="nav-items">
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </div>
      <div class="user-logo">R</div>
    </nav>
  </header>

  <RouterView />
</template>

<style scoped>
.wrapper {
  margin: 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.help-text {
  align-self: start;
}
header {
  border-bottom: 1px solid lightgray;
  border-radius: 8px;
}

.logo {
  display: block;
  /* margin: 0 auto 2rem; */
}

.user-logo {
  width: 40px;
  height: 40px;
  background-color: lightseagreen;
  border-radius: 20px;
  font-size: 18px;
  font-weight: 600;
  color: aliceblue;
  text-align: center;
  line-height: 40px;
}

nav {
  display: flex;
  width: 100%;
  font-size: 12px;
  text-align: center;
  justify-content: space-between;
  background-color: var(--color-background-mute);
  align-items: center;
  overflow: hidden;
  padding: 0.5rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  align-self: center;
  font-size: medium;
  /* border-left: 1px solid var(--color-border); */
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  .logo {
    /* margin: 0 1rem 0 0; */
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    /* margin-left: -1rem; */
    font-size: 1rem;
    padding: 0.5rem;
    /* margin-top: 1rem; */
  }
}
</style>
