<script setup lang="ts">
import { shuffle } from "@/utils/shuffle";
import { computed, reactive, watch } from "vue";
import Card from "./Card.vue";
import VictoryModal from "./VictoryModal.vue";

const symbols = [
  {
    name: "1",
    image: "",
  },
  {
    name: "2",
    image: "",
  },
  {
    name: "3",
    image: "",
  },
  {
    name: "4",
    image: "",
  },
];

const state = reactive({
  cards: [],
  matchedCards: new Set<number>(),
  openedCards: new Set<number>(),
  moves: 0,
});

const numPairs = computed(() => state.cards.length / 2);
const matches = computed(() => state.matchedCards.size / 2);
const twoCardsOpened = computed(() => state.openedCards.size === 2);
const isVictory = computed(() => matches.value === numPairs.value);

const startNewGame = () => {
  state.moves = 0;
  state.matchedCards.clear();
  state.openedCards.clear();
  state.cards = shuffle([...symbols, ...symbols]);
};

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

startNewGame();
</script>

<template>
  <section class="game">
    <h1 class="name">Memory game</h1>

    <div class="controls">
      <p>Moves: {{ state.moves }}</p>
      <p>Matches: {{ matches }} / {{ numPairs }}</p>
    </div>

    <ul class="board">
      <Card
        v-for="(card, index) in state.cards"
        :key="index"
        :name="card.name"
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
  grid-template-columns: repeat(6, 1fr);
  gap: 0.75rem;
  width: fit-content;
  margin-inline: auto;
  margin-bottom: 5rem;
}

.reset-button {
  margin-inline: auto;
  text-transform: capitalize;
  color: var(--white);
  background-color: var(--accent);
}
</style>
