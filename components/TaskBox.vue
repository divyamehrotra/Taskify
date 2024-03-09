<template>
  <div>
    <div class="board">
      <div
        v-for="(column, columnIndex) in columns"
        :key="columnIndex"
        class="column"
        @dragover.prevent="handleDragOver(columnIndex)"
        @drop="handleDrop(columnIndex)"
      >
        <h2>{{ column.name }} ({{ column.tasks.length }})</h2>
        <div
          class="tasks"
          v-for="task in column.tasks"
          :key="task.id"
          draggable
          @dragstart="handleDragStart(task, columnIndex)"
        >
          {{ task.name }}
        </div>
        <input
          v-model="newTaskName"
          type="text"
          placeholder="Add new task"
          @keyup.enter="addTask(columnIndex)"
        />
        <button @click="addTask(columnIndex)">  Add Task</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      columns: [
        { name: "To Do", tasks: [] },
        { name: "In Progress", tasks: [] },
        { name: "Done", tasks: [] }
      ],
      draggedTask: null,
      newTaskName: ""
    };
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
      }
      this.draggedTask = null;
    },
    addTask(columnIndex) {
      if (this.newTaskName.trim() !== "") {
        this.columns[columnIndex].tasks.push({
          id: Date.now(),
          name: this.newTaskName.trim()
        });
        this.newTaskName = "";
      }
    }
  }
};
</script>

<style>
.board {
  display: flex;
}

.column {
  margin: 0 20px;
}

.tasks {
  /* min-height: 100px; */
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
