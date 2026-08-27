<template>
  <div class="task-item">
    <span class="task-id"># {{ task?.id }}</span>
    <span class="task-id"># {{ task?.title }}</span>

    <button class="btn-toggle" @click="toggleDone">
      {{ btnToggleText }}
    </button>
    <button class="btn-remove" @click="removeTask">Remover</button>
  </div>
</template>
<script>
export default {
  name: "TaskItem",
  props: {
    task: {
      type: Object,
      required: true,
    },
  },
  methods: {
    toggleDone() {
      this.$emit("toggle-done", this.task.id);
    },
    removeTask() {
      this.$emit("remove-task", this.task.id);
    },
  },
  computed: {
    btnToggleText() {
      return this.task?.done ? "Desfazer" : "Concluir";
    },
  },

  beforeMount() {
    console.log("TaskItem será montado!");
  },
  mounted() {
    console.log("TaskItem: " + this.task.title + " foi montada!");
  },
  beforeUnmount() {
    console.log("A tarefa :" + this.task.title + " será desmontada.");
  },
  unmounted() {
    console.log(" A tarefa :" + this.task.title + " foi desmontada.");
  },
};
</script>
<style>
.task-item {
  display: flex;
  align-items: center;
  padding: 5px;
  margin-bottom: 8px;
  border-radius: 4px;
  background-color: white;
  border: 1px solid #ddd;
  transition: all 0.3s;
}
.task-id {
  font-weight: bold;
  margin-right: 10px;
  color: #7f8c8d;
}
.task-status {
  flex-grow: 1;
  width: 90px;
  text-align: left;
}
.btn-toggle {
  background-color: #f39c12;
  color: #fff;
  margin-right: 5px;
  width: 90px;
  text-align: center;
}

.btn-toggle:hover {
  background-color: #e67e22;
}
.btn-remove {
  background-color: #e74c3c;
  color: #fff;
  width: 90px;
  text-align: center;
}
.btn-remove:hover {
  background-color: #c0392b;
}
</style>
