<template>
  <div class="container">
    <h1>Jogo da Velha</h1>

    <div class="board">
      <div
        v-for="(cell, index) in board"
        :key="index"
        class="cell"
        @click="makeMove(index)"
      >
        {{ cell }}
      </div>
    </div>

    <p v-if="winner">Vencedor: {{ winner }} 🎉</p>
    <p v-else>Vez do jogador: {{ currentPlayer }}</p>

    <button @click="resetGame">Reiniciar</button>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const board = ref(Array(9).fill(""));
const currentPlayer = ref("X");
const winner = ref(null);

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
      return;
    }
  }
}

function makeMove(index) {
  if (board.value[index] || winner.value) return;

  board.value[index] = currentPlayer.value;

  checkWinner();

  if (!winner.value) {
    currentPlayer.value =
      currentPlayer.value === "X" ? "O" : "X";
  }
}

function resetGame() {
  board.value = Array(9).fill("");
  currentPlayer.value = "X";
  winner.value = null;
}
</script>

<style>
.container {
  text-align: center;
  font-family: Arial,
