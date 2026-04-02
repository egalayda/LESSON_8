<template>
  <div>
    <h1 class="page-title">About</h1>
    <div class="about-card">
      <span class="mdi mdi-information-outline about-icon"></span>
      <p>This dashboard was built with <strong>Vue 3</strong>, <strong>Vite</strong>, <strong>TypeScript</strong>, <strong>Vue Router</strong>, <strong>Chart.js</strong>, and <strong>Material Design Icons</strong>.</p>
    </div>
  </div>
</template>

<style scoped>
.page-title { font-size: 24px; font-weight: 700; margin-bottom: 24px; color: #1e293b; }
.about-card {
  background: #fff;
  border-radius: 12px;
  padding: 32px;
  box-shadow: 0 1px 4px rgba(0,0,0,0.07);
  display: flex;
  align-items: center;
  gap: 20px;
  font-size: 15px;
  line-height: 1.6;
  color: #475569;
}
.about-icon { font-size: 40px; color: #38bdf8; flex-shrink: 0; }
</style>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
