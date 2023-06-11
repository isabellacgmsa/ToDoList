<style scoped src="./Checklist.css"></style>
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
        <div v-if="task.editing">
          <input type="text" v-model="task.editedText" @keyup.enter="saveTask(task)" class="edit-input" />
          <button @click="cancelEditing(task)" class="cancel-button">Cancelar</button>
          <button @click="saveTask(task)" class="save-button">Salvar</button>
        </div>
        <div v-else class="task-content">
          <input type="checkbox" v-model="task.completed" class="checkbox" />
          <span :class="{ completed: task.completed }" @click="editTask(task)">{{ task.text }}</span>
          <button @click="removeTask(index)" class="remove-button">Remover</button>
        </div>
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
        this.tasks.push({ text: this.newTask, completed: false, editing: false, editedText: '' });
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
    editTask(task) {
      task.editing = true;
      task.editedText = task.text;
    },
    cancelEditing(task) {
      task.editing = false;
    },
    saveTask(task) {
      if (task.editedText.trim() !== '') {
        task.text = task.editedText;
      }
      task.editing = false;
    },
  },
};
</script>
