<template>
  <div class="container">
    <h1>Jogo da Velha</h1>

    <div class="board">
      <div
        v-for="(cell, index) in board"
        :key="index"
        class="cell"
        :class="getCellClass(index)"
        @click="makeMove(index)"
      >
        {{ cell }}
      </div>
    </div>

    <p v-if="winner">🎉 Vencedor: {{ winner }}</p>
    <p v-else-if="isDraw">🤝 Empate!</p>
    <p v-else>Vez do jogador: {{ currentPlayer }}</p>

    <button @click="resetGame">Reiniciar</button>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const board = ref(Array(9).fill(""));
const currentPlayer = ref("X");
const winner = ref(null);
const winningLine = ref([]);

const winPatterns = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6],
];

function checkWinner() {
  for (const pattern of winPatterns) {
    const [a, b, c] = pattern;

    if (
      board.value[a] &&
      board.value[a] === board.value[b] &&
      board.value[a] === board.value[c]
    ) {
      winner.value = board.value[a];
      winningLine.value = pattern;
      return;
    }
  }
}

function makeMove(index) {
  if (board.value[index] || winner.value) return;

  board.value[index] = currentPlayer.value;

  checkWinner();

  if (!winner.value) {
    currentPlayer.value = currentPlayer.value === "X" ? "O" : "X";
  }
}

const isDraw = computed(() => {
  return board.value.every(cell => cell !== "") && !winner.value;
});

function resetGame() {
  board.value = Array(9).fill("");
  currentPlayer.value = "X";
  winner.value = null;
  winningLine.value = [];
}

function getCellClass(index) {
  return {
    win: winningLine.value.includes(index),
    pop: board.value[index] !== ""
  };
}
</script>

<style>
.container {
  text-align: center;
  font-family: Arial, sans-serif;
  margin-top: 40px;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  gap: 5px;
  justify-content: center;
  margin: 20px 0;
}

.cell {
  width: 100px;
  height: 100px;
  background: #eee;
  font-size: 2rem;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  user-select: none;
  transition: all 0.2s ease;
}

/* ✨ animação ao jogar */
.cell.pop {
  animation: pop 0.2s ease-in-out;
}

/* 🏆 destaque da vitória */
.cell.win {
  background: #4caf50;
  color: white;
  font-weight: bold;
  transform: scale(1.05);
}

/* hover leve */
.cell:hover {
  background: #ddd;
}

/* botão */
button {
  margin-top: 10px;
  padding: 8px 16px;
  cursor: pointer;
}

/* animação */
@keyframes pop {
  0% { transform: scale(0.5); }
  100% { transform: scale(1); }
}
</style>
