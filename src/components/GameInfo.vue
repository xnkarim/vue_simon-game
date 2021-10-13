<template>
  <div class="game-info">
    <h2>
      Раунд:
      <span data-round="0">{{round}}</span>
    </h2>
    <button
      @click="$emit('start-game')"
      :class="{isBtnDisabled:'disabled-btn'}"
      data-action="start"
    >Start</button>
    <p v-if="isFailed">
      Извините, но вы проиграли после
      <span>{{lastRound}}</span> раундов!
    </p>
  </div>
</template>
<script>
export default {
  props: {
    status: String,
    round: Number,
    lastRound: Number
  },
  computed: {
    isFailed() {
      return this.status === "failed";
    },
    isBtnDisabled() {
      return this.status === "play" || this.status === "run";
    }
  }
};
</script>
<style scoped>
.disabled-btn {
  user-select: none;
}
</style>