<script setup>
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <div class="">
    <h1 class="text-center text-white bg-primary p-1">{{ title}}</h1>
    <div class="container">
      <div class="row">
        <div class="col">
          <input type="text" class="form-control" v-model="newTask" v-on:keyup.enter="addTask">
        </div>
        <div class="col">
          <input type="button" value="Add" class="btn btn-info" @click="addTask">
        </div>
        <div class="col">
          <button class="btn btn-danger" @click="deleteTask()">Delete</button>
        </div>
      </div>

      <div class="" v-if="filterTask.length > 0">
        <div class="row">
          <div class="col fs-3 mb-4">Tasks</div>
          <div class="col-2 fs-3 mb-4">Done</div>
        </div>
        <div class="row" v-for="(task, index) in filterTask" :key="index">
          <div class="col" :class="task.done ? 'delete': ''">{{ task.action }}</div>
          <div class="col-2">
            <input type="checkbox" v-model="task.done">
          </div>
        </div>
      </div>

      <div class="alert alert-warning" v-else>There is no data</div>

      <div class="row text-center bg-success py-2 mt-3 text-white d-flex align-items-center">
        <h5 class="col">Hide Complete Tasks</h5>
        <input type="checkbox" class="col form-check" v-model="hideCompleted">
      </div>
    </div>
  </div>

</template>
<script>
  export default {
    name: "App",
    data: ()=>({
      title: "My_Lists",
      hideCompleted: false,
      newTask: "",
      tasks: [

      ]
    }),
    computed: {
      filterTask(){
        return this.hideCompleted ? this.tasks.filter((v)=> !v.done) : this.tasks;
      }
    },
    methods: {
      addTask(){
        if (this.newTask === ""){
            return alert("please add new tasks");
        }
        this.tasks.push({
          action: this.newTask,
          done: false
        })
        this.storeData();
        this.newTask = '';
      },
      deleteTask(){
        this.tasks = this.tasks.filter((v)=> !v.done);
        this.storeData();
      },
      storeData(){
        localStorage.setItem("myLocalTasks", JSON.stringify(this.tasks));
      }
    },
    mounted() {
      let data = localStorage.getItem("myLocalTasks");
      if (data !== null){
        this.tasks = JSON.parse(data);
      }
    }
  }
</script>
<style>
  .delete{
    text-decoration: line-through;
  }
</style>