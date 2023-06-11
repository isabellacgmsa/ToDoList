<template>
  <div class="checklist">
    <div class="header">
      <h2>Minha To-Do List</h2>
      <button @click="markAllCompleted" v-if="showMarkAllButton && !allTasksCompleted">Marcar Todos</button>
      <button @click="markAllIncomplete" v-if="showMarkAllButton && allTasksCompleted">Desmarcar Todos</button>
    </div>
    <input type="text" v-model="newTask" placeholder="Nova tarefa" class="input-field" />
    <button @click="addTask" class="add-button">Adicionar</button>

    <ul class="task-list">
      <li v-for="(task, index) in tasks" :key="index" class="task">
        <input type="checkbox" v-model="task.completed" class="checkbox" />
        <span :class="{ completed: task.completed }">{{ task.text }}</span>
        <button @click="removeTask(index)" class="remove-button">Remover</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: [],
    };
  },
  computed: {
    allTasksCompleted() {
      return this.tasks.length > 0 && this.tasks.every((task) => task.completed);
    },
    showMarkAllButton() {
      return this.tasks.length > 0;
    },
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== '') {
        this.tasks.push({ text: this.newTask, completed: false });
        this.newTask = '';
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
    markAllCompleted() {
      this.tasks.forEach((task) => {
        task.completed = true;
      });
    },
    markAllIncomplete() {
      this.tasks.forEach((task) => {
        task.completed = false;
      });
    },
  },
};
</script>

<style scoped>
.checklist {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  background-color: #212121;
  color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
}

.input-field {
  display: block;
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  color: #ffffff;
  background-color: #424242;
}

.add-button {
  display: block;
  width: 100%;
  padding: 8px;
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.task-list {
  list-style-type: none;
  padding: 0;
}

.task {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
}

.checkbox {
  margin-right: 8px;
}

.completed {
  text-decoration: line-through;
}

.remove-button {
  margin-left: auto;
  background-color: #f44336;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
