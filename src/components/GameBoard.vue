<script setup lang="ts">
import { shuffle } from "@/utils/shuffle";
import { computed, onMounted, reactive, watch } from "vue";
import Card from "./Card.vue";
import VictoryModal from "./VictoryModal.vue";

const BOARD_SIZE = [4, 6, 8];

const state = reactive({
  selectedSize: 4,
  cards: [],
  matchedCards: new Set<number>(),
  openedCards: new Set<number>(),
  moves: 0,
});

const numPairs = computed(() => state.cards.length / 2);
const matches = computed(() => state.matchedCards.size / 2);
const twoCardsOpened = computed(() => state.openedCards.size === 2);
const isVictory = computed(() => matches.value === numPairs.value);

const fetchImages = async (count: number) => {
  const uniqueIds = Array.from({ length: count }, (_, i) => i + 10);
  return uniqueIds.map((id) => ({
    image: `https://picsum.photos/200/200?random=${id}`,
    name: id,
  }));
};

const startNewGame = async () => {
  const images = await fetchImages(state.selectedSize);
  state.moves = 0;
  state.matchedCards.clear();
  state.openedCards.clear();
  state.cards = shuffle([...images, ...images]);
};

onMounted(() => startNewGame());

const flipCard = (index: number) => {
  state.moves += 1;
  state.openedCards.add(index);
};

const getCardStatus = (index: number) => {
  if (state.openedCards.has(index)) return "opened";
  if (state.matchedCards.has(index)) return "matched";
  return "closed";
};

watch(twoCardsOpened, (value) => {
  if (!value) {
    return;
  }

  setTimeout(() => {
    state.openedCards.clear();
  }, 1000);

  const [firstIndex, secondIndex] = Array.from(state.openedCards.values());
  if (state.cards[firstIndex].name === state.cards[secondIndex].name) {
    state.matchedCards.add(firstIndex);
    state.matchedCards.add(secondIndex);
  }
});
</script>

<template>
  <section class="game">
    <h1 class="name">Memory game</h1>

    <div class="controls">
      <label for="size"
        >Board Size:
        <select v-model="state.selectedSize" @change="startNewGame">
          <option v-for="size in BOARD_SIZE" :key="size" :value="size">
            {{ size }} x {{ size }}
          </option>
        </select></label
      >

      <p>Moves: {{ state.moves }}</p>
      <p>Matches: {{ matches }} / {{ numPairs }}</p>
    </div>

    <ul
      class="board"
      :style="{
        gridTemplateColumns: `repeat(auto-fit, minmax(200px, 1fr))`,
      }"
    >
      <Card
        v-for="(card, index) in state.cards"
        :key="index"
        :image="card.image"
        :status="getCardStatus(index)"
        :disabled="twoCardsOpened"
        @click="flipCard(index)"
      />
    </ul>

    <button class="reset-button" @click="startNewGame">New game</button>

    <VictoryModal
      :moves="state.moves"
      :isGameOver="isVictory"
      @close="startNewGame"
    />
  </section>
</template>

<style scoped>
.game {
  display: flex;
  flex-direction: column;
  justify-content: center;
  min-height: inherit;
}

.name {
  margin-bottom: 1.5rem;
  font-size: 2rem;
  text-transform: uppercase;
  text-align: center;
}

.controls {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 2rem;
  margin-bottom: 1.5rem;
}

.board {
  display: grid;
  gap: 0.75rem;
  width: 75%;
  margin-inline: auto;
  margin-bottom: 5rem;
  justify-content: center;
  list-style: none;
}

.reset-button {
  margin-inline: auto;
  text-transform: capitalize;
  color: var(--white);
  background-color: var(--accent);
}
</style>
