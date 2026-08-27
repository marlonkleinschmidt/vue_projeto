<template>
  <div class="task-list-container">
    <h2 v-once>Lista de Tarefas</h2>

    <div class="controls">
      <button :class="btnAddClass" @click="handleShowForm">
        {{ btnAddText }}
      </button>
      <button class="btn-toggle-all">Marcar Todas</button>
      <button class="btn-clear">Limpar Concluídas</button>
    </div>

    <div v-if="showForm" class="add-task-container">
      <input
        v-model="newTaskTitle"
        type="text"
        placeholder="Digite o titulo da tarefa"
        class="task-input"
      />
      <button class="btn-add" @click="addTask">Adicionar</button>
    </div>

    <div class="tasks-container">
      <div class="pending-tasks">
        <h3 v-once>Tarefas Pendentes</h3>
        <p v-if="pendingTasks.length === 0">
          Nenhuma tarefa pendente no momento
        </p>
        <div v-else>
          <TaskItem
            v-for="task in pendingTasks"
            v-memo="[task.done, task.title]"
            :key="task.id"
            :task="task"
            @toggle-done="toggleTaskDone"
            @remove-task="removeTask"
          />
        </div>
      </div>

      <div class="completed-tasks">
        <h3 v-once>Tarefas Concluídas</h3>
        <p v-if="completedTasks.length === 0">
          Nenhuma tarefa concluída no momento
        </p>
        <div v-else>
          <TaskItem
            v-for="task in completedTasks"
            v-memo="[task.done, task.title]"
            :key="task.id"
            :task="task"
            @toggle-done="toggleTaskDone"
            @remove-task="removeTask"
          />
        </div>
      </div>
    </div>
    <span>Componente de contador</span>
    <div>
      <h3>Resumo</h3>
      <p v-if="tasks.length === 0">Você ainda não possui tarefas.</p>
      <p v-else-if="pendingTasks.length > 0 && completedTasks.length === 0">
        Você tem {{ pendingTasks.length }} tarefas pendentes.
      </p>
      <p v-else-if="completedTasks.length > 0 && pendingTasks.length === 0">
        Todas as tarefas foram concluídas!
      </p>
      <p v-else>
        Você tem {{ pendingTasks.length }} pendente(s) e
        {{ completedTasks.length }} concluídas!
      </p>
    </div>
  </div>
</template>

<script>
import TaskItem from "./TaskItem.vue";

export default {
  name: "TaskList",
  components: { TaskItem },
  data() {
    return {
      tasks: [],

      newTaskTitle: "",
      showForm: false,
    };
  },
  beforeCreate() {
    console.log("beforeCreate chamado!");
    console.log("this.tasks ainda é: ", this.tasks);
  },
  created() {
    console.log("created chamado!");
    const savedTasks = localStorage.getItem("taskList");
    if (savedTasks) {
      try {
        this.tasks = JSON.parse(savedTasks);
      } catch (e) {
        console.error("Erro ao carregar tarefas do LocalStorage", e);
      }
    }
    console.log("Agora this.tasks existe: ", this.tasks);
  },
  beforeMount() {
    console.log("beforeMount foi chamado!");
  },
  mounted() {
    console.log("mounted foi chamado!");
  },
  beforeUpdate() {
    console.log("TaskList -> beforeUpdate.");
  },
  updated() {
    console.log("TaskList -> updated");
  },
  methods: {
    removeTask(taskId) {
      this.tasks = this.tasks.filter((task) => task.id != taskId);
    },
    toggleTaskDone(taskId) {
      const task = this.tasks.find((t) => t.id === taskId);
      if (task) task.done = !task.done;
    },

    addTask() {
      if (this.newTaskTitle.trim() === "") return;

      this.tasks.push({
        id: Date.now(),
        title: this.newTaskTitle.trim(),
        done: false,
      });

      this.newTaskTitle = "";
      this.showForm = false;
    },
    handleShowForm() {
      this.showForm = !this.showForm;
    },
  },
  watch: {
    tasks: {
      handler(newVal) {
        try {
          localStorage.setItem("taskList", JSON.stringify(newVal));
        } catch (e) {
          console.log("Erro ao salvar tarefas no LocalStorage", e);
        }

        // const message = `Lista de Tasks mudou! Itens: ${newVal.length}`;
        // this.logWatch(message);
        // if (oldVal) {
        //   const modified = newVal.filter((n) => {
        //     const oldTask = oldVal.find((o) => o.id === n.id);
        //     return oldTask && JSON.stringify(n) !== JSON.stringify(oldTask);
        //   });
        //   if (modified.length > 0) {
        //     const modifyMsg = `Tarefas modificadas: ${modified
        //       .map((t) => t.id)
        //       .join(", ")}`;
        //     this.logWatch(modifyMsg);
        //   }
        // }
      },
      deep: true,
      immediate: false,
    },
  },
  computed: {
    completedTasks() {
      return this.tasks.filter((task) => task.done);
    },
    pendingTasks() {
      return this.tasks.filter((task) => !task.done);
    },
    btnAddText() {
      return this.showForm ? "Fechar" : "Adicionar Nova Tarefa";
    },
    btnAddClass() {
      return this.showForm ? "btn-clear" : "btn-add";
    },
  },
};
</script>
<style>
.task-list-container {
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px;
}
h2 {
  color: #3498db;
  margin-bottom: 20px;
  padding-bottom: 10px;
  border-bottom: 2px solid #f0f0f0;
  text-align: center;
}
.controls {
  display: flex;
  gap: 10px;
  margin-bottom: 30px;
  flex-wrap: wrap;
  justify-content: center;
}

button {
  padding: 10px 20px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: bold;
  transition: all 0.3s;
  font-size: 14px;
}
.btn-add {
  background-color: #2ecc71;
  color: white;
}

.btn-add:hover {
  background-color: #27ae60;
  transform: translateY(-2px);
}

.btn-toggle-all {
  background-color: #3498db;
  color: white;
}

.btn-toggle-all:hover {
  background-color: #2980b9;
  transform: translateY(-2px);
}

.btn-clear {
  background-color: #e74c3c;
  color: white;
}
.btn-clear:hover {
  background-color: #c0392b;
  transform: translateY(-2px);
}
.tasks-container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 30px;
  margin-bottom: 30px;
}
.pending-tasks,
.completed-tasks {
  padding: 20px;
  border-radius: 8px;
}

.pending-tasks {
  background-color: #f9f9f9;
  border: 2px solid #eee;
}

.completed-tasks {
  background-color: #f0fff0;
  border: 2px solid #d4edda;
}

.pending-tasks h3,
.completed-tasks h3 {
  margin-top: 0;
  margin-bottom: 15px;
  color: #2c3e50;
  text-align: center;
}
.watch-output {
  background-color: #2c3e50;
  color: white;
  padding: 15px;
  border-radius: 6px;
}
.log-container {
  max-height: 200px;
  overflow-y: auto;
  background-color: #1a252f;
  padding: 10px;
  border-radius: 4px;
  margin-top: 10px;
  font-family: monospace;
}
.add-task-container {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-bottom: 20px;
}

.task-input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 6px;
  width: 300px;
}
</style>
