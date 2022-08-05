<template>
  <main>
    <div class="container">
      <img id="my-pic" src="@/assets/brian.jpg" alt="Brian Eft" />
      <section id="name-and-links">
        <span id="my-name">Brian&nbsp;Eft</span>
        <nav>
          <a href="mailto:hi@brianeft.com">Email</a>
          <a href="https://linkedin.com/in/brianeft">LinkedIn</a>
          <a href="https://github.com/nefty">GitHub</a>
        </nav>
      </section>
    </div>
  </main>
</template>

<style scoped>
.container {
  margin: 5rem 0;
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}

#my-pic {
  max-width: 280px;
  padding: 1rem;
  border-radius: 100%;
}

#name-and-links {
  margin: 0 1rem;
}

#name-and-links nav {
  display: flex;
  padding-bottom: 2.25rem;
  justify-content: center;
}

#name-and-links nav a {
  padding: 0 0.5rem;
}

#my-name {
  font-size: 3rem;
  font-weight: 300;
  text-align: center;
}

@media (min-width: 1024px) {
  nav a {
    font-size: 0.75rem;
  }
}
</style>
