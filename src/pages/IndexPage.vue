<template>
  <q-page class="page column justify-center">
    <div class="header row justify-center">
      <div>
        <div class="header row justify-center">
          <h3>Tic Tac Toe.</h3>
        </div>
        <div>
          <q-btn class="btn-player q-ma-md" :label="data.scorePlayer1" />
          <q-btn class="btn-player q-ma-md" :label="data.scorePlayer2" />
        </div>
        <div class="header row justify-center" v-if="data.winner">
          <b>Ha ganado {{ data.winner }} !!!</b>
        </div>
        <div class="header row justify-center" v-if="data.draw">
          <b>{{ data.draw }} !!!</b>
        </div>
      </div>
    </div>
    <div class="body row justify-center">
      <div class="container" v-if="!data.winner && !data.draw">
        <div class="box" v-for="(item, index) in data.board" :key="index">
          <q-btn
            class="btn"
            @click="updateArray(index)"
            v-if="item.state === 'unable'"
          />
          <p class="player" v-else>{{ data.board[index].value }}</p>
        </div>
      </div>
    </div>
    <div class="row justify-center">
      <q-btn
        class="btn-reset q-ma-xl"
        label="Reset Game"
        @click="resetBoard"
        v-if="!data.winner && !data.draw"
      />
      <q-btn
        class="btn-reset q-ma-xl"
        label="rematch!!!"
        @click="rematch"
        v-else
      />
    </div>
  </q-page>
</template>

<script setup>
import { reactive, onMounted } from "vue";
import confetti from "canvas-confetti";

const player = ["❌", "⚪"];
const combinations = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6],
];

let data = reactive({
  player: "",
  board: [
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
  ],
  player1: [],
  player2: [],
  turn: 0,
  scorePlayer1: "",
  scorePlayer2: "",
  score1: 0,
  score2: 0,
  winner: null,draw:null
});

function updateArray(index) {
  if (data.turn == 0) {
    data.board[index].state = "able";
    data.board[index].value = player[0];
    data.player1.push(index);
    data.turn++;
  } else {
    data.board[index].state = "able";
    data.board[index].value = player[1];
    data.player2.push(index);
    data.turn--;
  }

  if (data.player1.length >= 3) checkWinner(data.player1, "Player 1");
  if (data.player2.length >= 3) checkWinner(data.player2, "Player 2");
}

function checkWinner(player, winner) {
  for (let index = 0; index < combinations.length; index++) {
    let isFounded = combinations[index].every((comb) => player.includes(comb));
    if (isFounded) updateScore(winner);
  }

  if((data.player1.length==5 || data.player2.length==5) && !data.winner) data.draw="Draw"
}

function updateScore(winner) {
  if (winner === "Player 1")
    data.score1++, (data.winner = "Player 1"), confetti();
  if (winner === "Player 2")
    data.score2++, (data.winner = "Player 2"), confetti();

  const text1 = "Player 1 ❌:";
  data.scorePlayer1 = text1.concat(data.score1);

  const text2 = "Player 2 ⚪:";
  data.scorePlayer2 = text2.concat(data.score2);

}

function resetBoard() {
  data.turn = 0;
  (data.player1 = []),
    (data.player2 = []),
    (data.score1 = 0),
    (data.score2 = 0);
  updateScore("");
  data.board = [
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
  ];
}

function rematch() {
  data.winner = null;
  data.draw=null
  data.board = [
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
    { value: null, state: "unable" },
  ];
  (data.player1 = []), (data.player2 = []);
}

onMounted(() => {
  updateScore("");
});

</script>

<style>
.page {
  background-color: rgb(127, 190, 182);
}

.container {
  width: 300px;
  height: 300px;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  font-size: 60px;
  text-align: center;
  padding: 0%;
  justify-content: center;
}

.box {
  background-color: transparent;
  border-radius: 10px;
  display: grid;
  text-align: center;
  height: 96px;
  width: 96px;
}

@media (min-width: 700px) {
  .container {
    width: 500px;
    height: 500px;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    font-size: 100px;
    text-align: center;
    padding: 0%;
    justify-content: center;
  }

  .box {
    background-color: transparent;
    border-radius: 10px;
    display: grid;
    text-align: center;
    height: 160px;
    width: 160px;
  }
}

.btn {
  background-color: rgb(241, 245, 248);
  width: 100%;
  height: 100%;
  border-radius: 10px;
  margin: auto;
}

.btn-player {
  background-color: rgb(31, 179, 179);
}

.btn-reset {
  background-color: rgb(204, 72, 72);
}

.player {
  margin: auto;
}

h3 {
  color: rgb(66, 70, 70);
}
</style>
