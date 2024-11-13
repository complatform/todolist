<template>
  <div class="w-6/7 lg:ml-48 text-center shadow-lg lg:w-2/3 mt-20 rounded-md pb-10 md:pl-5 md:pr-5">
    <div class="lg:ml-10">
      <div class="flex ml-24 lg:ml-96 md:ml-80 font-serif text-2xl">
        <h1 class="text-green-400 text-center">Todo List</h1>
        <i class="fa-solid fa-list mt-1 ml-3 text-green-400 text-center"></i>
      </div>
    </div>
    <div>
      <input
        type="text"
        placeholder="add your task"
        class="shadow-lg px-7 mt-5 focus:outline-blue-300 py-3 rounded-md lg:w-96"
        v-model="taskList.tasks"
      />
      <input
        type="date"
        class="shadow-lg px-7 mt-5 focus:outline-blue-300 py-3 rounded-md ml-4"
        v-model="taskList.date"
      />

      <button
        class="bg-green-200 px-8 py-2 rounded-md ml-4 font-serif shadow-lg" 
        v-if="!taskUpdate"
        @click="addTask"
      >
        Add Task
      </button>
      <button
        class="bg-green-200 px-8 py-2 rounded-md lg:ml-4 ml-1 font-serif shadow-lg" 
        v-if="taskUpdate"
        @click="updateTask"
      >
        Update Task
      </button><br />
      <div v-for="showTask in showTasks" :key="showTask.id">
        <div class="flex mt-10 lg:ml-36 md:ml-36 ml-2 mr-2 justify-between lg:mr-5 lg:w-2/3 md:w-2/3">
          <p class="font-serif lg:text-2xl md:text-xl">{{ showTask.tasks }}</p>
          <p class="font-serif lg:text-2xl ml-3 md:text-xl">{{ showTask.date }}</p>
             <button
            class="shadow-lg lg:text-2xl px-3 py-1 rounded-md text-green-500"
            v-if="showTask.isComplated"
          >
            <i class="fa-solid fa-check lg:text-3xl md:text-xl"></i>
          </button>
          <i class="fa-regular fa-trash-can lg:text-3xl md:text-xl px-7 py-2 rounded-md text-red-400 ml-2 shadow-lg" @click="deleteTask(showTask.id)" v-if="!showTask.isComplated"></i>
          <i class="fa-solid fa-pen-to-square lg:text-3xl md:text-xl px-7 py-2 rounded-md text-blue-300 ml-2 shadow-lg" @click="editTask(showTask.id)" v-if="!showTask.isComplated"></i>
          <i class="fa-solid fa-xmark lg:text-3xl px-7 py-2 rounded-md text-green-700 ml-2 shadow-lg" @click="complateTask(showTask.id)" v-if="!showTask.isComplated"></i>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      taskList: {
        tasks: "",
        date: "",
        isComplated: false,
      },
      taskID: 1,
      showTasks: [],
      taskUpdate: false,
    };
  },
  mounted() {
    const Item = localStorage.getItem('task');
    const ItemParse = JSON.parse(Item);
    if (ItemParse) {
      this.showTasks.push(...ItemParse);
      this.taskID = this.showTasks.length ? Math.max(...this.showTasks.map(task => task.id)) + 1 : 1; 
    }
  },
  methods: {
    addTask() {
      if (!this.taskList.tasks || !this.taskList.date) return; 
      const newTask = { ...this.taskList, id: this.taskID++ };
      this.showTasks.push(newTask);
      this.saveTasks(); 
      this.resetTaskList();
    },
    deleteTask(id) {
      this.showTasks = this.showTasks.filter(show => show.id !== id);
      this.saveTasks();
    },
    complateTask(id) {
      const task = this.showTasks.find(task => task.id === id);
      if (task) {
        task.isComplated = true; 
        this.saveTasks(); 
      }
    },
    editTask(id) {
      const taskToEdit = this.showTasks.find(task => task.id === id);
      if (taskToEdit) {
        this.taskList.tasks = taskToEdit.tasks;
        this.taskList.date = taskToEdit.date;
        this.taskID = id; 
        this.taskUpdate = true;
      }
    },
    updateTask() {
      if (this.taskID) {
        const taskIndex = this.showTasks.findIndex(task => task.id === this.taskID);
        if (taskIndex !== -1) {
          this.showTasks[taskIndex] = { ...this.taskList, id: this.taskID }; 
          this.saveTasks(); 
          this.resetTaskList();
        }
      }
    },
    saveTasks() {
      localStorage.setItem('task', JSON.stringify(this.showTasks));
    },
    resetTaskList() {
      this.taskList.tasks = "";
      this.taskList.date = "";
      this.taskUpdate = false; 
    }
  },
};
</script>

