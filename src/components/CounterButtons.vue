<template>
  <div class="buttons">
    <h3>Valor de count: {{ countValue }}</h3>
    <MyButton title="Soma" @update="$emit('change-count', 'increment', 1)" />
    <MyButton
      :disabled="!canDecrement"
      title="Subtração"
      @update="$emit('change-count', 'decrement', 1)"
    />
    <MyButton
      :disabled="!canReset"
      title="Resetar"
      @update="$emit('change-count', 'reset')"
    />
  </div>
</template>
<script>
import MyButton from "./MyButton.vue";
export default {
  name: "CounterButtons",
  emits: ["change-count"],
  components: {
    MyButton,
  },
  props: {
    countValue: {
      type: Number,
      required: true,
    },
  },
  computed: {
    canDecrement() {
      return this.countValue > -10;
    },

    canReset() {
      return this.countValue !== 0;
    },
  },
};
</script>
<style>
.buttons {
  display: flex;
  gap: 30px;
}
</style>
