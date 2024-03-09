<template>
    <div>
      <div class="board">
        <div
          v-for="(column, columnIndex) in columns"
          :key="columnIndex"
          class="column"
        >
          <h2>{{ column.name }} ({{ column.tasks.length }})</h2>
          <div
            class="tasks"
            v-for="task in column.tasks"
            :key="task.id"
            draggable
            @dragstart="handleDragStart(task, columnIndex)"
            @dragover.prevent
            @drop="handleDrop(columnIndex)"
          >
            {{ task.name }}
          </div>
          <input
            v-model="newTaskNames[columnIndex]"
            type="text"
            :placeholder="'Add New Task'"
            @keyup.enter="addTask(columnIndex)"
          />
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        columns: [],
        draggedTask: null,
        newTaskNames: ['', '', '']
      };
    },
    mounted() {
      // Load data from local storage on component mount
      const storedData = localStorage.getItem('kanbanData');
      if (storedData) {
        this.columns = JSON.parse(storedData);
      } else {
        // Initialize columns if no data is stored
        this.columns = [
          { name: "To Do", tasks: [] },
          { name: "In Progress", tasks: [] },
          { name: "Done", tasks: [] }
        ];
      }
    },
    methods: {
      handleDragStart(task, columnIndex) {
        this.draggedTask = { ...task, columnIndex };
      },
      handleDrop(targetColumnIndex) {
        if (!this.draggedTask) return;
        const { name, columnIndex } = this.draggedTask;
        if (columnIndex !== targetColumnIndex) {
          this.columns[targetColumnIndex].tasks.push({ id: Date.now(), name });
          this.columns[columnIndex].tasks = this.columns[columnIndex].tasks.filter(
            task => task.id !== this.draggedTask.id
          );
          // Save data to local storage after drop
          localStorage.setItem('kanbanData', JSON.stringify(this.columns));
        }
        this.draggedTask = null;
      },
      addTask(columnIndex) {
        if (this.newTaskNames[columnIndex].trim() !== "") {
          this.columns[columnIndex].tasks.push({
            id: Date.now(),
            name: this.newTaskNames[columnIndex].trim()
          });
          this.newTaskNames[columnIndex] = "";
          // Save data to local storage after adding task
          localStorage.setItem('kanbanData', JSON.stringify(this.columns));
        }
      }
    }
  };
  </script>
  
  <style>
  .board {
    display: flex;
    margin-top: 30px;
    justify-content: center;
  }
  
  .column {
    margin: 0 20px;
  }
  
  .tasks {
    border: 1px solid #ccc;
    padding: 10px;
    margin-top: 10px;
  }
  
  .task {
    background-color: #f8f8f8;
    border: 1px solid #ddd;
    padding: 5px 10px;
    margin: 5px 0;
    cursor: pointer;
  }
  
  input[type="text"] {
    padding: 5px;
    margin-top: 5px;
  }
  </style>
  