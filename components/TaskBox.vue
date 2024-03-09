<template>
    <div class="task-box"
         @drop="handleDrop"
         @dragover.prevent
    >
      <h2>{{ title }}</h2>
      <ul>
        <li v-for="task in tasks" :key="task.id"
            draggable
            @dragstart="handleDragStart(task)"
        >
          {{ task.title }}
        </li>
      </ul>
      <div class="add-task">
        <input type="text" v-model="newTaskTitle" placeholder="Enter task title" />
        <button @click="addTask">Add Task</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['title', 'tasks'],
    data() {
      return {
        newTaskTitle: ''
      };
    },
    methods: {
      addTask() {
        if (this.newTaskTitle.trim() === '') return;
        this.$emit('add-task', this.newTaskTitle);
        this.newTaskTitle = '';
      },
      handleDragStart(task) {
        event.dataTransfer.setData('text/plain', JSON.stringify(task));
      },
      handleDrop() {
        const task = JSON.parse(event.dataTransfer.getData('text/plain'));
        this.$emit('drop-task', task, this.title);
      }
    }
  };
  </script>
  
  <style scoped>
  .task-box {
    border: 1px solid #ccc;
    padding: 10px;
    margin-bottom: 20px;
  }
  .add-task {
    margin-top: 10px;
  }
  .add-task input {
    padding: 5px;
    margin-right: 10px;
  }
  </style>
  