<script setup lang="ts">
import ThemeSwitcher from "./components/ThemeSwitcher.vue";
</script>

<template>
  <main class="wrapper">
    <h1>MEMORY GAME</h1>
    <ThemeSwitcher />
  </main>
</template>

<style scoped>
.wrapper {
  position: relative;
  display: grid;
  place-content: center;
  min-height: 100dvh;
}
</style>
