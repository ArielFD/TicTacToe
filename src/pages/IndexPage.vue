<template>
  <q-page class="flex flex-center">
    <h3>Tic Tac Toe</h3>
    <div class="container">
      <div class="box" v-for="(item, index) in data.board" :key="index">
        <q-btn
          class="btn"
          @click="updateArray(index)"
          v-if="item.state === 'unable'"
        />
        <p class="player" v-else>{{ data.board[index].value }}</p>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { reactive, onMounted } from "vue";

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
  if (data.player1.length >= 3) checkWinner(data.player1,"Player 1")
  if((data.player2.length >= 3)) checkWinner(data.player2,"Player 2")
}

function checkWinner(player,winner) {
    for (let index = 0; index < combinations.length; index++) {
      let isFounded = combinations[index].every((ai) => player.includes(ai));
      if(isFounded) alert("ha Ganado: "+winner)
    }
}

</script>

<style>
.flex {
  background-color: rgb(127, 190, 182);
}

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

.btn {
  background-color: rgb(241, 245, 248);
  width: 100%;
  height: 100%;
  border-radius: 10px;
  margin: auto;
}

.player {
  margin: auto;
}
</style>
