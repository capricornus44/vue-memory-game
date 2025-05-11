<script setup lang="ts">
import GameBoard from "./components/GameBoard.vue";
import ThemeSwitcher from "./components/ThemeSwitcher.vue";
</script>

<template>
  <main class="wrapper">
    <GameBoard />
    <ThemeSwitcher />
  </main>
</template>

<style scoped>
.wrapper {
  position: relative;
  min-height: 100dvh;
}
</style>
