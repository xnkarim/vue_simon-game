<template>
  <div
    @click="clicked"
    class="tile"
    :class="[tile.active ? 'active' : '', tile.color]"
  ></div>
</template>
<script>
export default {
  props: {
    tile: {
      type: Object,
      required: true
    },
    status: {
      type: String
    }
  },
  computed: {
    id() {
      return this.tile.id;
    },
    isActive() {
      return this.tile.active;
    }
  },
  watch: {
    isActive(val) {
      if (val == true) this.play();
    }
  },
  methods: {
    clicked() {
      if (this.status != "play") return;
      this.$emit("tile-clicked", this.id);
      this.tile.active = true;
      this.play();
    },
    play() {
      this.playAudio();
      setTimeout(() => {
        this.tile.active = false;
      }, 150);
    },
    playAudio() {
      const name = this.id+1;
      const audio = new Audio(`sounds/${name}.ogg`);
      audio.addEventListener("canplaythrough", () => {
        audio.play();
      });
    }
  }
};
</script>