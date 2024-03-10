<template>
    <div>
        <div class="board">
            <div v-for="(column, columnIndex) in columns" :key="columnIndex" class="column">
                <h2 class="column-title">{{ column.name }} ({{ column.tasks.length }})</h2>
                <draggable v-model="columns[columnIndex].tasks"
                           :group="{ name: 'tasks', put: true }"
                           :item-key="task => task.id" 
                           @change="saveData"
                           @start="dragStart"
                           @end="dragEnd"
                           @add="dragAdd">

                    <template v-slot:item="{ element }">
                        <div :key="element.id"
                            :class="{ 'task': true, 'dragging': dragIndex === columnIndex }">
                            {{ element.name }}
                            <button @click="deleteTask(columnIndex, element.id)" class="delete-button">
                                <i class='bx bxs-message-alt-x'></i>
                            </button>
                        </div>
                    </template>
                </draggable>
                <input v-model="newTaskNames[columnIndex]" type="text" :placeholder="'+ New '"
                    @keyup.enter="addTask(columnIndex)" />
            </div>
        </div>
    </div>
    <footer class="bg-pink-300 text-black py-4 fixed bottom-0 w-full">
      <div class="max-w-screen-xl mx-auto px-4 sm:px-6 lg:px-8">
        <p class="text-center">© 2024 Taskify. All rights reserved. Made with love ❤️</p>
      </div>
    </footer>
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
            newTaskNames: ['', '', ''],
            dragIndex: -1 // Initialize dragIndex to -1
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
        },
        dragStart(event) {
            // Set dragIndex to the index of the column being dragged from
            this.dragIndex = event.oldIndex;
        },
        dragEnd() {
            // Reset dragIndex when dragging ends
            this.dragIndex = -1;
        },
        dragAdd(event) {
            // Set dragIndex to the index of the column being dragged to
            this.dragIndex = event.newIndex;
        }
    }
};
</script>

<style>
.board {
    display: flex;
    margin-top: 60px;
    justify-content: center;
}

.column {
    margin: 0 20px;
}

.column-title {
    color: rgb(255, 255, 255);
    background-color: #b87e9d;
    border-radius: 2px;
    text-align: center;
}

.task {
    background-color: #f4e3ed;
    border: 1px solid #ddd;
    padding: 5px 10px;
    margin: 8px;
    cursor: pointer;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    transition: background-color 0.3s; /* Add transition for smooth color change */
}

.task.dragging {
    background-color: #f8f8f8; /* Change background color while dragging */
}

.delete-button {
    cursor: pointer;
}

.delete-button i {
    color: #ba0967;
    font-size: large;
}

.delete-button i:hover{
    color: black;
}

@media only screen and (max-width: 768px) {
    .board {
        flex-wrap: wrap;
    }
}
</style>
