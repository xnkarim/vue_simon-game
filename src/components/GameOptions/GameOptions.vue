<template>
  <div class="game-options">
    <h2>{{title}}</h2>
    <div class="radio__wrapper">
      <div v-for="option in options" :key="option.value">
        <input
          type="radio"
          :name="option.name"
          :value="option.value"
          :checked="option.value == value"
          @change="$emit('input', option.value)"
        />
        {{ option.label }}
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    title: String,
    value: Number,
    options: Array
  },
  components: {},
  data: () => ({})
};
</script>
<style scoped>
.radio__wrapper {
  display: flex;
  flex-direction: column;
  align-items: baseline;
  margin-top: 20px;
}
</style>