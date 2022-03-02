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
const themeState = ref("");

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

const SwitchTheme = () => {
  if (document.getElementById("gameBox").classList.contains("dark")) {
    document.getElementById("gameBox").classList.remove("dark");
  } else {
    document.getElementById("gameBox").classList.add("dark");
  }
};




const PickSide = (a) => {
  player.value = a;
  console.log(player.value);
};
</script>

<template>
  <div id="gameBox" :class="`duration-300`">
    <main class="pt-8 text-center bg-light-bg dark:bg-dark-bg min-h-screen">
      <div class="inline-flex">
        <h1 class="text-3xl font-bold uppercase text-light-tx1 dark:text-dark-tx2">
          Tic Tac Toe Game
        </h1>
        <!-- <button
          @click="SwitchTheme()"
          :class="`absolute right-4 px-4 py-1 bg-light-tx2 text-white rounded-full uppercase font-bold hover:bg-light-tx1 duration-300`"
        >
          Toggle
        </button> -->
        <div
          class="inline-flex absolute right-8 mt-1.5 items-center space-x-2 mx-auto"
        >
          <span class="text-xs font-extralight text-light-tx1 dark:text-dark-tb">Light </span>

          <div>
            <input @click="SwitchTheme()" type="checkbox" name="" id="checkbox" class="hidden" />
            <label for="checkbox" class="cursor-pointer">
              <div
                class="w-9 h-5 flex items-center bg-light-tx2 dark:bg-dark-tx2 rounded-full p2"
              >
                <div class="switch-ball w-4 h-4 bg-white rounded-full shadow"></div>
              </div>
            </label>
          </div>

          <span class="text-xs font-semibold text-light-tb dark:text-dark-tx2">Dark</span>
        </div>
      </div>
      <h3 class="text-xl font-bold mt-4 mb-4 text-light-tb dark:text-dark-tb">
        Player {{ player }}`s turn
      </h3>

      <div class="flex flex-col items-center mb-8">
        <div v-for="(row, x) in board" :key="x" class="flex">
          <div
            v-for="(cell, y) in row"
            :key="y"
            @click="MakeMove(x, y)"
            :class="`border-2 border-light-tb dark:border-dark-tb w-24 h-24 hover:bg-light-ext flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${
              cell === 'X' ? 'text-light-tx2' : 'text-blue-400'
            }`"
          >
            {{ cell === "X" ? "close" : cell === "O" ? "circle" : "" }}
          </div>
        </div>
      </div>
      <h2 v-if="winner" class="text-6xl font-bold mb-8 text-light-tx dark:text-dark-tb">
        Player '{{ winner }}' wins
      </h2>
      <div v-if="!gameStarted">
        <h3 class="text-xl mb-5 font-bold uppercase text-light-tx2">
          Choose a side
        </h3>
        <button
          :class="`w-20 mx-16 my-2 bg-light-tx2 text-light-tx rounded-full uppercase font-bold hover:bg-pink 400 duration-300`"
          @click="PickSide('X')"
        >
          X
        </button>
        <button
          :class="`w-20 mx-16 my-2 bg-light-tx2 text-light-tx  rounded-full uppercase font-bold hover:bg-pink 400 duration-300`"
          @click="PickSide('O')"
        >
          O
        </button>
      </div>
      <button
        v-if="winner"
        @click="ResetGame"
        :class="`px-4 py-2 bg-light-tx1 text-white dark:bg-dark-tx2 rounded-full uppercase font-bold hover:bg-light-tx1 duration-300`"
      >
        Start a New Game
      </button>
    </main>
  </div>
</template>

<style>
#checkbox:checked + label .switch-ball {
  background-color: white;
  transform: translateX(18px);
  transition: transform 0.3s linear;
}

#checkbox:not(:checked) + label .switch-ball {
  background-color: white;
  transform: translateX(2px);
  transition: transform 0.3s linear;
}

  * {
    @apply transition-colors duration-700;
  }

</style>
