<template>
  <div class="checklist p-4">
    <div class="header mb-4">
      <h2 class="text-2xl font-bold">To-Do List</h2>
<br>
      <button
        @click="markAllCompleted"
        v-if="showMarkAllButton && !allTasksCompleted"
        class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600"
      >
        Mark All
      </button>
      <button
        @click="markAllIncomplete"
        v-if="showMarkAllButton && allTasksCompleted"
        class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600"
      >
        Unmark All
      </button>
    </div>
    <div class="flex mb-4">
      <input
        type="text"
        v-model="newTask"
        placeholder="New task"
        class="flex-grow border border-gray-300 rounded-md px-4 py-2 focus:outline-none"
      />
      <button
        @click="addTask"
        class="ml-4 px-4 py-2 bg-green-500 text-white rounded-md hover:bg-green-600"
      >
        Add
      </button>
    </div>

    <ul class="task-list">
      <li v-for="(task, index) in tasksFiltered" :key="index" class="task mb-4 p-4 bg-gray rounded-md shadow">
        <div v-if="task.editing" class="flex items-center mb-2">
          <input
            type="text"
            v-model="task.editedText"
            @keyup.enter="saveTask(task)"
            class="flex-grow border border-gray-300 rounded-md px-4 py-2 focus:outline-none"
          />
          <button
            @click="cancelEditing(task)"
            class="ml-2 px-4 py-2 bg-red-500 text-white rounded-md hover:bg-red-600"
          >
            Cancel
          </button>
          <button
            @click="saveTask(task)"
            class="ml-2 px-4 py-2 bg-green-500 text-white rounded-md hover:bg-green-600"
          >
            Save
          </button>
        </div>
        <div v-else class="task-content flex items-center">
          <input type="checkbox" v-model="task.completed" class="mr-2" />
          <span :class="{ completed: task.completed }" @click="editTask(task)" class="flex-grow">
            {{ task.text }}
          </span>
          <select v-model="task.priority" class="border border-gray-300 rounded-md px-2 py-1 focus:outline-none">
            <option value="low">Low</option>
            <option value="medium">Medium</option>
            <option value="high">High</option>
          </select>
          <button
            @click="removeTask(index)"
            class="ml-4 px-4 py-2 bg-red-500 text-white rounded-md hover:bg-red-600"
          >
            Remove
          </button>
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

<style scoped>
.completed {
  text-decoration: line-through;
}
</style>