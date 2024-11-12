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
        name=""
        id=""
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
        @click="addTask"
      >
        Add Task</button
      ><br />
      <div v-for="showTask in showTasks" :key="showTask.id">
        <div class="flex mt-10 lg:ml-36 md:ml-36 ml-2 mr-2 justify-between lg:mr-5 lg:w-2/3 md:w-2/3">
          <p v-if="showTasks" class="font-serif lg:text-2xl md:text-xl">
            {{ showTask.tasks }}
          </p>
          <p v-if="showTasks" class="font-serif lg:text-2xl ml-3 md:text-xl">
            {{ showTask.date }}
          </p>
          <button
            class="shadow-lg lg:text-2xl px-3 py-1 rounded-md text-green-500"
            v-if="showTask.isComplated"
          >
            <i class="fa-solid fa-check lg:text-3xl md:text-xl"></i>
          </button>

          <!-- <button
            class=" bg-red-600 px-7 py-2 rounded-md text-white ml-2" v-if="!showTask.isComplated"
            @click="deleteTask(showTask.id)"
          >
            Delete
          </button> -->
          <i
            class="fa-regular fa-trash-can lg:text-3xl md:text-xl px-7 py-2 rounded-md text-red-400 ml-2 shadow-lg"
            @click="deleteTask(showTask.id)"
            v-if="!showTask.isComplated"
          ></i>
          <!-- <button class=" bg-blue-300 px-8 py-2 rounded-md text-white ml-2" @click="complateTask(showTask.id)" v-if="!showTask.isComplated">
            edit
          </button> -->
          <!-- <i
            class="fa-solid fa-pen-to-square lg:text-3xl md:text-xl px-7 py-2 rounded-md text-blue-300 ml-2 shadow-lg"
            v-if="!showTask.isComplated"
            @click="editTask(showTask.id)"
          ></i> -->
          <!-- <button class=" bg-green-700 px-5 py-2 rounded-md text-white ml-2" @click="complateTask(showTask.id)" v-if="!showTask.isComplated">
            Complate
          </button> -->
          <i
            class="fa-solid fa-xmark lg:text-3xl px-7 py-2 rounded-md text-green-700 ml-2 shadow-lg"
            @click="complateTask(showTask.id)"
            v-if="!showTask.isComplated"
          ></i>
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
        date: new Date(),
        isComplated: false,
      },
      taskID: 1,
      showTasks: [],
    };
  },
  methods: {
    addTask() {
      this.showTasks.push({ ...this.taskList, id: this.taskID++ });
      console.log(this.showTasks);
      (this.taskList.tasks = ""), (this.taskList.date = null);
    },
    deleteTask(id) {
      console.log(id);
      this.showTasks = this.showTasks.filter((show) => show.id !== id);
    },
    complateTask(id) {
      this.showTasks.filter((task) => {
        if (task.id == id) {
          task.isComplated = true;
          console.log(task.isComplated);
        }
      });
    },
    editTask(id){
this.showTasks.filter((task)=>{
  if(task.id == id){
    task.tasks=this.taskList.tasks,
    task.date=null
  }
})
    }
  },
};
</script>

<style>
</style>