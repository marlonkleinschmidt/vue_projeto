<template>
  <div class="container-box">
    <TaskList />
    <!-- <h2 :style="{ color: changeCountCount }">{{ count }}</h2>
    <CounterButtons :count-value="count" @change-count="handleCountChange" />
    <p :style="{ color: changeCountCount }">
      {{ changeState }}
    </p> -->
  </div>
</template>

<script>
import TaskList from "./components/TaskList.vue";

export default {
  name: "App",
  components: { TaskList },
  data() {
    return {
      count: Number(localStorage.getItem("count")) || 0,
    };
  },
  methods: {
    handleCountChange(action, value = 1) {
      switch (action) {
        case "increment":
          this.count += value;
          break;
        case "decrement":
          this.count -= value;
          break;
        case "reset":
          this.count = 0;
          break;
      }
    },
  },
  computed: {
    changeCountCount() {
      if (this.count === 0) return "black";
      if (this.count > 0) return "green";
      return "red";
    },
    changeState() {
      if (this.count === 0) return "É zero";
      if (this.count > 0) return "é positivo!";
      return "é negativo";
    },
  },
  watch: {
    count(newValue) {
      localStorage.setItem("count", newValue);
    },
    changeCountCount(newValue) {
      alert(`é ${newValue}`);
    },
  },
};
</script>

<style>
.container-box {
  display: flex;
  flex-direction: row;
  gap: 10px;
  border: 1px solid rgb(221, 218, 218);
  border-radius: 12px;
  background-color: white;
  padding: 10px;
  justify-content: center;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
