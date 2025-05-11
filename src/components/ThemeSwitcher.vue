<script setup lang="ts">
import { ref, onMounted } from "vue";

const theme = ref("light");

const toggleTheme = () => {
  theme.value = theme.value === "light" ? "dark" : "light";
  document.documentElement.style.colorScheme = theme.value;
};

onMounted(() => {
  document.documentElement.style.colorScheme = theme.value;
});
</script>

<template>
  <button @click="toggleTheme">
    Switch to {{ theme === "light" ? "Dark" : "Light" }} Mode
  </button>
</template>

<style scoped>
button {
  position: fixed;
  top: 1.5rem;
  right: 1.5rem;
  color: var(--white);
  background-color: var(--accent);
}
</style>
