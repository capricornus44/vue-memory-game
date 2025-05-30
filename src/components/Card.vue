<script setup lang="ts">
defineProps<{
  image: string;
  status: "opened" | "closed" | "matched";
  disabled: boolean;
}>();
</script>

<template>
  <li
    class="card"
    :class="{
      flipped: status === 'opened' || status === 'matched',
      matched: status === 'matched',
      disabled,
    }"
  >
    <div class="card-inner">
      <div class="card-front"></div>

      <div class="card-back">
        <img :src="image" />
      </div>
    </div>
  </li>
</template>

<style scoped>
.card {
  height: 150px;
  width: 100%;
  perspective: 1000px;
  cursor: pointer;
}

.card.disabled {
  pointer-events: none;
}

.card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  transition: transform var(--transition);
}

.card.flipped .card-inner {
  transform: rotateY(180deg);
}

.card-front,
.card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  display: grid;
  place-content: center;
  border-radius: var(--radius-sm);
  box-shadow: var(--shadow);
}

.card-front {
  border: 2px solid var(--accent);
  background-color: light-dark(var(--white), var(--black));
  background-image: repeating-linear-gradient(
      45deg,
      var(--accent) 0,
      var(--accent) 10px,
      transparent 10px,
      transparent 20px
    ),
    repeating-linear-gradient(
      -45deg,
      var(--accent) 0,
      var(--accent) 10px,
      transparent 10px,
      transparent 20px
    );
  background-size: 200% 200%;
}

.card-back {
  border: 2px solid var(--accent);
  transform: rotateY(180deg);
  overflow: hidden;
}

.card-back img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.card.matched .card-back {
  border: 2px dashed var(--accent);
  filter: blur(1px);
}
</style>
