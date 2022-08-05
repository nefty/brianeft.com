<script setup>
import { RouterLink, RouterView } from "vue-router";
</script>

<template>
  <header>
    <span id="header-name"><RouterLink to="/">Brian&nbsp;Eft</RouterLink></span>
    <nav>
      <RouterLink to="/">Home</RouterLink>
      <RouterLink to="/projects">Projects</RouterLink>
      <RouterLink to="/blog">Blog</RouterLink>
    </nav>
  </header>

  <RouterView />

  <footer>
    <p>
      <span class="footer-item">© Brian Eft 2022</span>
      <span class="footer-item">
        <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>
        License</span
      >
    </p>
  </footer>
</template>

<style scoped>
#header-name a {
  color: var(--color-text);
  transition: 0s;
}

#header-name a:hover {
  background-color: transparent;
  font-weight: bold;
}

#header-name {
    font-size: 1.25rem;
  overflow: visible;
}

</style>
