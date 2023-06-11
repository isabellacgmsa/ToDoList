<style scoped src="./Checklist.css"></style>
<template>
  <div class="checklist">
    <div class="header">
      <h2>My To-Do List</h2>
      <button @click="markAllCompleted" v-if="showMarkAllButton && !allTasksCompleted">Mark All</button>
      <button @click="markAllIncomplete" v-if="showMarkAllButton && allTasksCompleted">Unmark All</button>
    </div>
    <input type="text" v-model="newTask" placeholder="New task" class="input-field" />
    <button @click="addTask" class="add-button">Add</button>

    <ul class="task-list">
      <li v-for="(task, index) in tasksFiltered" :key="index" class="task">
        <div v-if="task.editing">
          <input type="text" v-model="task.editedText" @keyup.enter="saveTask(task)" class="edit-input" />
          <button @click="cancelEditing(task)" class="cancel-button">Cancel</button>
          <button @click="saveTask(task)" class="save-button">Save</button>
        </div>
        <div v-else class="task-content">
          <input type="checkbox" v-model="task.completed" class="checkbox" />
          <span :class="{ completed: task.completed }" @click="editTask(task)">{{ task.text }}</span>
          <select v-model="task.priority" class="priority-select">
            <option value="low">Low</option>
            <option value="medium">Medium</option>
            <option value="high">High</option>
          </select>
          <button @click="removeTask(index)" class="remove-button">Remove</button>
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
    tasksFiltered() {
    const priorityOrder = {
      high: 0,
      medium: 1,
      low: 2,
    };

    const sortedTasks = this.tasks.slice().sort((a, b) => {
      return priorityOrder[a.priority] - priorityOrder[b.priority];
    });

    return sortedTasks;
  },
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== '') {
        this.tasks.push({ text: this.newTask, completed: false, editing: false, editedText: '', priority: 'low' });
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