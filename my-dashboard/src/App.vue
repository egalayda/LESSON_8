<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <div class="app-layout">
    <aside class="sidebar">
      <div class="sidebar-brand">
        <span class="mdi mdi-view-dashboard"></span>
        <span class="brand-name">My Dashboard</span>
      </div>
      <nav class="sidebar-nav">
        <RouterLink to="/" class="nav-item">
          <span class="mdi mdi-home"></span>
          <span>Home</span>
        </RouterLink>
        <RouterLink to="/about" class="nav-item">
          <span class="mdi mdi-information"></span>
          <span>About</span>
        </RouterLink>
      </nav>
    </aside>
    <main class="main-content">
      <RouterView />
    </main>
  </div>
</template>

<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: 'Inter', sans-serif; background: #f0f2f5; color: #333; }
</style>

<style scoped>
.app-layout {
  display: flex;
  min-height: 100vh;
}
.sidebar {
  width: 220px;
  background: #1e293b;
  color: #fff;
  display: flex;
  flex-direction: column;
  padding: 24px 0;
  flex-shrink: 0;
}
.sidebar-brand {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 0 20px 24px;
  font-size: 18px;
  font-weight: 700;
  border-bottom: 1px solid #334155;
}
.sidebar-brand .mdi { font-size: 26px; color: #38bdf8; }
.sidebar-nav {
  display: flex;
  flex-direction: column;
  gap: 4px;
  margin-top: 16px;
  padding: 0 12px;
}
.nav-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 12px;
  border-radius: 8px;
  color: #94a3b8;
  text-decoration: none;
  font-size: 14px;
  transition: background 0.2s, color 0.2s;
}
.nav-item .mdi { font-size: 20px; }
.nav-item:hover, .nav-item.router-link-active {
  background: #334155;
  color: #fff;
}
.main-content {
  flex: 1;
  padding: 32px;
  overflow-y: auto;
}
</style>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
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
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
