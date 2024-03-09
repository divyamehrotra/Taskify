<template>
    <div class="task-manager mt-5">
      <div class="task-box" @drop="handleDrop('notStarted')" @dragover.prevent>
        <h2>Not Started ({{ notStartedTasks.length }})</h2>
        <ul>
          <li v-for="task in notStartedTasks" :key="task.id" draggable @dragstart="handleDragStart(task)">
            {{ task.title }}
          </li>
        </ul>
        <div class="add-task">
          <input type="text" v-model="newNotStartedTask" placeholder="Enter task title" />
          <button @click="addTask('notStarted')">Add Task</button>
        </div>
      </div>
  
      <div class="task-box" @drop="handleDrop('inProgress')" @dragover.prevent>
        <h2>In Progress ({{ inProgressTasks.length }})</h2>
        <ul>
          <li v-for="task in inProgressTasks" :key="task.id" draggable @dragstart="handleDragStart(task)">
            {{ task.title }}
          </li>
        </ul>
        <div class="add-task">
          <input type="text" v-model="newInProgressTask" placeholder="Enter task title" />
          <button @click="addTask('inProgress')">Add Task</button>
        </div>
      </div>
  
      <div class="task-box" @drop="handleDrop('completed')" @dragover.prevent>
        <h2>Completed ({{ completedTasks.length }})</h2>
        <ul>
          <li v-for="task in completedTasks" :key="task.id" draggable @dragstart="handleDragStart(task)">
            {{ task.title }}
          </li>
        </ul>
        <div class="add-task">
          <input type="text" v-model="newCompletedTask" placeholder="Enter task title" />
          <button @click="addTask('completed')">Add Task</button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        notStartedTasks: [],
        inProgressTasks: [],
        completedTasks: [],
        newNotStartedTask: '',
        newInProgressTask: '',
        newCompletedTask: ''
      };
    },
    methods: {
      addTask(category) {
        let newTaskTitle = '';
        switch (category) {
          case 'notStarted':
            newTaskTitle = this.newNotStartedTask;
            this.notStartedTasks.push({ id: Date.now(), title: newTaskTitle });
            this.newNotStartedTask = '';
            break;
          case 'inProgress':
            newTaskTitle = this.newInProgressTask;
            this.inProgressTasks.push({ id: Date.now(), title: newTaskTitle });
            this.newInProgressTask = '';
            break;
          case 'completed':
            newTaskTitle = this.newCompletedTask;
            this.completedTasks.push({ id: Date.now(), title: newTaskTitle });
            this.newCompletedTask = '';
            break;
          default:
            break;
        }
      },
      handleDragStart(task) {
        event.dataTransfer.setData('text/plain', JSON.stringify(task));
      },
      handleDrop(category) {
        event.preventDefault();
        const data = JSON.parse(event.dataTransfer.getData('text/plain'));
        const task = { id: Date.now(), title: data.title };
        switch (category) {
          case 'notStarted':
            this.notStartedTasks.push(task);
            break;
          case 'inProgress':
            this.inProgressTasks.push(task);
            break;
          case 'completed':
            this.completedTasks.push(task);
            break;
          default:
            break;
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .task-manager {
    display: flex;
    justify-content: space-around;
  }
  .task-box {
    border: 1px solid #ccc;
    padding: 10px;
    margin-bottom: 20px;
  }
  .task-box ul {
    list-style-type: none;
    padding: 0;
  }
  .task-box ul li {
    margin-bottom: 5px;
    cursor: pointer;
  }
  .add-task input {
    padding: 5px;
    margin-right: 10px;
  }
  </style>
  