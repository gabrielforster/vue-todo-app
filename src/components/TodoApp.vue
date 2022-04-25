<template>
  <div class="container">
    <h1 class="title">Vue TODO App</h1>

    <div class="inputs">
      <input v-model="task" type="text" placeholder="Digite uma tarefa" class="task-input" v-on:keyup="handleKeyPress($event)" >
      <button class="btn" @click="submitTask"><span class="fa fa-check"></span></button>
    </div>
  
      <div class="tasks-table">
        <div class="task-body" v-for="(task, index) in tasks" :key="index">
          <p class="task-name" :class="{'finished': task.status === 'Feita'}" >{{firstLetterUpper(task.name)}}</p>
          <p
            @click="changeStatus(index)"
            class="task-status"
            :class="{
              'danger': task.status === 'A fazer',
              'good': task.status ===  'Feita'
              }"
          >
            {{firstLetterUpper(task.status)}}
          </p>
          <p class="task-edit" @click="editTask(index)" ><span class="fa fa-pen"></span></p>
          <p class="task-delete" @click="deleteTask(index)" ><span class="fa fa-trash"></span></p>
        </div>
      </div>

  </div>
</template>

<script>
export default {
  name: "TodoApp",
  
  data(){
    return{
      task: "",
      editedTask: null,
      tasksStatus: ["A fazer", "Feita"],
      tasks:[]
    }
  },

  created(){
    document.title = 'ToDo'
  },

  mounted(){
    if(localStorage.getItem('tasks')){
      this.tasks = JSON.parse(localStorage.getItem('tasks'))
    }
  },

  watch:{
    tasks:{
      handler(){
        localStorage.setItem('tasks', JSON.stringify(this.tasks))
      },
      deep: true,
    }
  },

  methods: {
    handleKeyPress(event){
      if(event.key === 'Enter') return this.submitTask()
    },

    submitTask(){
      if(this.task.length === 0) return

      if(this.editedTask === null){
        this.tasks.push({
          name: this.task,
          status: "A fazer",
        })
      }else{
        this.tasks[this.editedTask].name = this.task
        this.editedTask = null
      }
      
      this.task = ""
    },

    deleteTask(index){
      this.tasks.splice(index, 1)
    },

    editTask(index){
      this.task = this.tasks[index].name
      this.editedTask = index
    },

    changeStatus(index){
      let newIndex = this.tasksStatus.indexOf(this.tasks[index].status)
      if(++newIndex > 1) newIndex = 0
      this.tasks[index].status = this.tasksStatus[newIndex]
    },

    firstLetterUpper(status){
      return status.charAt(0).toUpperCase() + status.slice(1)
    }

  }

}
</script>

<style lang="scss">
  @import "../assets/styles.scss";
</style>