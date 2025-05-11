<script setup lang="ts">
defineProps<{
  moves: number;
  isGameOver: boolean;
}>();
</script>

<template>
  <Teleport to="body">
    <Transition name="modal">
      <div v-if="isGameOver" class="modal-backdrop">
        <div class="modal-content">
          <h2>Congratulations! 🎉</h2>
          <p>You won in {{ moves }} moves</p>
          <button class="modal-button" @click="$emit('close')">
            Play Again
          </button>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<style scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1000;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: var(--translucent);
  backdrop-filter: blur(4px);
}

.modal-content {
  max-width: 90%;
  width: 400px;
  padding: 2rem;
  border-radius: var(--radius-sm);
  text-align: center;
  background-color: light-dark(var(--white), var(--black));
  box-shadow: var(--shadow);
}

.modal-content h2 {
  margin-bottom: 1rem;
}

.modal-content p {
  margin-bottom: 1.5rem;
}

.modal-button {
  text-transform: capitalize;
  color: var(--white);
  background-color: var(--accent);
}

/* Modal transition animations */
.modal-enter-active,
.modal-leave-active {
  transition: opacity var(--transition);
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}
</style>
