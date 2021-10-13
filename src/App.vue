<template>
  <div id="app">
    <div class="wrapper">
      <h1>Simon</h1>
      <game-board :status.sync="status" :dificulty="dificulty" :round="round" @tile-clicked="tileClicked" />
      <game-info @start-game="startGame" :status="status" :round="round" :lastRound="lastRound" />
      <game-options title="Уровень сложности:" v-model="dificulty" :options="options" />
    </div>
  </div>
</template>

<script>
import GameBoard from "./components/GameBoard/GameBoard.vue";
import GameInfo from "./components/GameInfo.vue";
import GameOptions from "./components/GameOptions/GameOptions.vue";

export default {
  name: "App",
  components: {
    GameBoard,
    GameInfo,
    GameOptions
  },
  data: () => ({
    status: "disabled",
    round: 0,
    lastRound: 0,
    dificulty: 1500,
    options: [
      {
        label: "Легкий",
        name: "dificulty",
        value: 1500
      },
      {
        label: "Средний",
        name: "dificulty",
        value: 1000
      },
      {
        label: "Сложный",
        name: "dificulty",
        value: 400
      }
    ]
  }),
  watch: {
    status(val) {
      if (val === "win") this.nextRound();
      else if (val === "failed") this.failed();
    },
    dificulty(){
      // this.startGame();
    }
  },
  methods: {
    startGame() {
      this.round = 1;
      this.status = "run";
    },
    nextRound() {
      this.round++;
      this.status = "next";
    },
    failed() {
      this.lastRound = this.round;
      this.round = 0;
      this.status = "failed";
    },
    tileClicked() {}
  }
};
</script>

<style>
@import url("./assets/style.scss");

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
