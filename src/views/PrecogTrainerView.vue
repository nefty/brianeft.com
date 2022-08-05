<script setup></script>

<template>
  <main>
    <nav class="subnav">
      <RouterLink to="/precog-trainer">Precog Trainer</RouterLink>
      <RouterLink to="/precog-trainer/about">About</RouterLink>
    </nav>
    <RouterView></RouterView>
  </main>
</template>

<style>
.subnav {
  font-size: 1rem;
  text-align: center;
  justify-content: center;
  padding: 0;
}

@media (min-width: 1024px) {
  .subnav {
    font-size: 1.25rem;
  }
}
</style>
