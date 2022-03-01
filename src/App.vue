<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import { ref, computed } from "vue";

const player = ref("X");
const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const gameState = ref("");

const CalculateWinner = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];

  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
};
const isGameOn = (gameStarted) => {
  return gameStarted;
};
const winner = computed(() => CalculateWinner(board.value.flat()));
const gameStarted = computed(() => isGameOn(gameState.value));

const MakeMove = (x, y) => {
  if (!gameStarted.value) gameState.value = true;
  if (winner.value) return;
  if (board.value[x][y] !== "") return;

  board.value[x][y] = player.value;

  if (player.value === "X") player.value = "O";
  else player.value = "X";

  // player.value = player.value === "X" ? "0" : "X";
};

const ResetGame = () => {
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
  player.value = "X";
  gameState.value = false;
};

const PickSide = (a) => {
  player.value = a;
  console.log(player.value);
};
</script>

<template>
  <main class="pt-8 text-center dark:bg-gray-800 min-h-screen dark:text-white">
    <h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe Game</h1>
    <h3 class="text-xl mb-4">Player {{ player }}`s turn</h3>

    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <div
          v-for="(cell, y) in row"
          :key="y"
          @click="MakeMove(x, y)"
          :class="`border border-white w-24 h-24 hover:bg-teal-700 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${
            cell === 'X' ? 'text-pink-500' : 'text-blue-400'
          }`"
        >
          {{ cell === "X" ? "close" : cell === "O" ? "circle" : "" }}
        </div>
      </div>
    </div>
    <h2 v-if="winner" class="text-6xl font-bold mb-8">
      Player '{{ winner }}' wins
    </h2>
    <div v-if="!gameStarted">
      <h3 class="text-xl mb-5 font-bold uppercase">Choose a side</h3>
      <button :class="`w-20 mx-16 bg-pink-800 rounded-full uppercase font-bold hover:bg-pink 400 duration-300`" @click="PickSide('X')">X</button>
      <button :class="`w-20 mx-16 bg-pink-800 rounded-full uppercase font-bold hover:bg-pink 400 duration-300`" @click="PickSide('O')">O</button>
    </div>
    <button
      v-if="winner"
      @click="ResetGame"
      :class="`px-4 py-2 bg-pink-800 rounded-full uppercase font-bold hover:bg-pink-400 duration-300`"
    >
      Start a New Game
    </button>
  </main>
</template>

<style></style>
