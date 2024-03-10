<template>
    <div>
      <div class="board">
        <div v-for="(column, columnIndex) in columns" :key="columnIndex" class="column">
          <h2>{{ column.name }} ({{ column.tasks.length }})</h2>
          <draggable v-model="columns[columnIndex].tasks" :group="{ name: 'tasks', put: true }" @change="saveData">
            <template v-slot:item="{ element }">
              <div :key="element.id" class="task">
                {{ element.name }}
                <button @click="deleteTask(columnIndex, element.id)" class="delete-button">
                  <i class='bx bxs-message-alt-x'></i>
                </button>
              </div>
            </template>
          </draggable>
          <input v-model="newTaskNames[columnIndex]" type="text" :placeholder="'+' + column.name" @keyup.enter="addTask(columnIndex)" />
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import draggable from 'vuedraggable';
  
  export default {
    components: {
      draggable
    },
    data() {
      return {
        columns: [],
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
      saveData() {
        // Save data to local storage
        localStorage.setItem('kanbanData', JSON.stringify(this.columns));
      },
      addTask(columnIndex) {
        if (this.newTaskNames[columnIndex].trim() !== "") {
          this.columns[columnIndex].tasks.push({
            id: Date.now(),
            name: this.newTaskNames[columnIndex].trim()
          });
          this.newTaskNames[columnIndex] = "";
          this.saveData();
        }
      },
      deleteTask(columnIndex, taskId) {
        this.columns[columnIndex].tasks = this.columns[columnIndex].tasks.filter(task => task.id !== taskId);
        this.saveData();
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
  
  .task {
    background-color: #f8f8f8;
    border: 1px solid #ddd;
    padding: 5px 10px;
    margin: 5px 0;
    cursor: pointer;
  }
  
  .delete-button {
    height: 20px;
    z-index: 1;
    color: #000000;
    border: none;
    padding: 5px 10px;
    margin-left: 80px;
    cursor: pointer;
  }
  
  .delete-button i {
    z-index: 1;
    color: #000000;
  }
  </style>
  