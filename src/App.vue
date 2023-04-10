<template>
  <div class="to-do-list-container">
    <div class="to-do-list">
      <h1>Lista de Tarefas</h1>
      <div class="add-task-container">
        <input
          type="text"
          v-model="newTask"
          placeholder="Adicionar nova tarefa"
        />
        <button @click="addTask">Adicionar</button>
      </div>
      <ul>
        <li v-for="(task, index) in tasks" :key="index">
          <div class="task-container">
            <div class="checkbox-container">
              <input
                type="checkbox"
                :checked="task.completed"
                @change="task.completed = !task.completed"
                class="checkbox"
              />

              <span class="checkmark"></span>
            </div>
            <div class="task-text" :class="{ completed: task.completed }">
              {{ task.text }}
            </div>
            <button
              class="edit-task-button"
              @click="editTask(index)"
              v-if="!task.editing"
            >
              Editar
            </button>

            <div class="edit-task-container" v-if="task.editing">
              <input
                type="text"
                v-model="task.text"
                @blur="saveTask(index)"
                ref="editTaskInput"
              />
              <button @click="saveTask(index)">Salvar</button>
            </div>
            <div class="remove-task-container">
              <button @click="removeTask(index)">Excluir</button>
            </div>
          </div>
        </li>
      </ul>
      <div class="buttons-container">
        <button @click="completeAllTasks" v-if="!allTasksCompleted">
          Marcar todas como concluídas
        </button>
        <button @click="clearCompletedTasks" v-if="completedTasks.length > 0">
          Remover todas as concluídas
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [],
    };
  },
  computed: {
    allTasksCompleted() {
      return (
        this.tasks.length > 0 && this.tasks.every((task) => task.completed)
      );
    },
    completedTasks() {
      return this.tasks.filter((task) => task.completed);
    },
  },
  methods: {
    addTask() {
      if (this.newTask.trim() === "") return;
      this.tasks.push({
        text: this.newTask.trim(),
        completed: false,
        editing: false,
      });
      this.newTask = "";
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
    completeTask(index) {
      this.tasks[index].completed = true;
    },

    editTask(index) {
      this.tasks[index].editing = true;
      this.$nextTick(() => this.$refs.editTaskInput[index].focus());
    },
    saveTask(index) {
      if (this.tasks[index].text.trim() === "") {
        this.removeTask(index);
      } else {
        this.tasks[index].editing = false;
      }
    },

    cancelEdit(index) {
      this.tasks[index].editing = false;
    },

    completeAllTasks() {
      this.tasks.forEach((task) => (task.completed = true));
    },
    clearCompletedTasks() {
      this.tasks = this.tasks.filter((task) => !task.completed);
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Fira+Code:wght@300;400;500;600;700&display=swap");

* {
  font-family: "Fira Code", monospace;
}

body {
  background: linear-gradient(to right, #242424 50%, #484848 50%);
  overflow: hidden;
}

.completed {
  text-decoration: line-through;
}

.checkbox-container {
  width: 20px;
  height: 20px;
  margin-right: 10px;
}

.checkbox {
  width: 20px;
  height: 20px;
  margin: 0;
}

.to-do-list-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.to-do-list {
  background-color: #41b883;
  color: #fff;
  padding: 20px;
  max-width: 500px;
  width: 100%;
}

.to-do-list h1 {
  text-align: center;
  margin-top: 0;
  margin-bottom: 20px;
  background: #000000;
}

.to-do-list input[type="text"] {
  padding: 10px;
  border-radius: 5px;
  border: none;
  margin-right: 10px;
  width: 75.5%;
}

.task-text {
  max-width: 500px;
  height: auto;
  word-break: break-word;
}

.to-do-list button {
  padding: 10px;
  border-radius: 5px;
  border: none;
  background-color: #35495e;
  color: #fff;
  font-weight: 500;
  cursor: pointer;
}

.buttons-container {
  column-gap: 20px;
  display: flex;
}

.to-do-list ul {
  list-style: none;
  padding: 0;
}

.to-do-list ul li:nth-child(odd) {
  background-color: #33a06f;
}

.to-do-list ul li:nth-child(even) {
  background-color: #155f3e;
}

.to-do-list li {
  display: flex;
  align-items: center;
}

.task-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 10px;
}

.to-do-list li input[type="checkbox"] {
  margin-right: 10px;
}

.to-do-list li button {
  padding: 5px;
  border-radius: 5px;
  border: none;
  background-color: #ed3c50;
  color: #fff;
  margin-left: auto;
  cursor: pointer;
}

.edit-task-container{
  display: flex;
  height: 27px;
}

.remove-task-container {
  margin-left: 10px;
}
</style>
