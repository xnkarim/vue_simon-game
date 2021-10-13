<template>
  <div class="game-board">
    <div class="simon">
      <tile
        v-for="tile in tiles"
        :tile="tile"
        :key="tile.id"
        :status="status"
        @tile-clicked="tileClicked"
        @change-activity="tile.active = $event"
      />
    </div>
  </div>
</template>
<script>
import Tile from "./Tile.vue";
export default {
  props: {
    status: String,
    isDisabled: {
      type: Boolean,
      default: false
    },
    round: Number,
    dificulty: {
      dificulty: Number,
      required: true
    }
  },
  components: {
    Tile
  },
  data: () => ({
    sequence: [],
    userSequence: [],
    tiles: [
      {
        id: 0,
        color: "red",
        active: false
      },
      {
        id: 1,
        color: "blue",
        active: false
      },
      {
        id: 2,
        color: "yellow",
        active: false
      },
      {
        id: 3,
        color: "green",
        active: false
      }
    ]
  }),
  watch: {
    status(val) {
      if (val === "run") this.start();
      if (val == "next") this.run();
    }
  },
  methods: {
    tileClicked(id) {
      this.userSequence.push(id);
      if (this.isFailed()) {
        this.$emit("update:status", "failed");
        this.userSequence = [];
        this.sequence = [];
      } else if (this.sequence.length === this.userSequence.length) {
        this.$emit("update:status", "win");
        this.userSequence = [];
      }
    },
    isFailed() {
      const length = this.userSequence.length;
      return this.sequence[length - 1] !== this.userSequence[length - 1];
    },
    async play() {
      for (const item of this.sequence) {
        console.log(item);
        const tile = this.tiles[item];
        await this.playSound(tile);
      }
      this.$emit("update:status", "play");
    },
    async playSound(item) {
      return new Promise(resolve => {
        setTimeout(() => {
          item.active = true;
          resolve();
        }, this.dificulty);
      });
    },
    start(){
      this.sequence = [];
      this.userSequence = [];
      this.run();
    },
    run() {
      let rnd = this.getRandom();
      this.sequence.push(rnd);
      this.play();
    },
    getRandom() {
      return Math.floor(Math.random() * 4);
    }
  }
};
</script>
<style scoped>
.disabled {
  pointer-events: none;
}
</style>