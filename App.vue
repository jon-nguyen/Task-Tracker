<template>
  <div class = "container">
    <Header v-on:toggle-add-task="toggleAddTask" title ="Task Tracker" v-bind:showAddTask="showAddTask"/>
    <div v-if="showAddTask">
    <AddTask v-on:add-task="addTask"/>
    </div>
    <Tasks v-on:toggle-reminder="toggleReminder" v-on:delete-task="deleteTask" v-bind:tasks = "tasks"/>
  </div>
</template>

<script>
import Header from './components/Header'
import Tasks from './components/Tasks'
import AddTask from './components/AddTask'

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask,
  },
  data() {
    return{ 
      tasks: [],
      showAddTask: false
    }
  },
  methods: {
    //task is from newTask in AddTask.vue (this.$emit('add-task', newTask))
    addTask(task){
      // adds new tasks onto previous tasks
      this.tasks = [...this.tasks, task]
    },
    deleteTask(id){
      // going through, finding tasks without id, and returning tasks without id
      this.tasks = this.tasks.filter((task)=>task.id !== id)
    },
    toggleReminder(id){
      // for each task, check if task.id == id, then return array of objects where selected task has initial task properties (...task), but reminder has opposite task.reminder or task
      this.tasks = this.tasks.map((task) => task.id == id ? {...task, reminder: !task.reminder} : task) 
    },
    toggleAddTask(){
      this.showAddTask= !this.showAddTask
    }
  },
  created(){
    this.tasks = [
      {
        id:1,
        text: 'Doctors Appointment',
        day: 'March 1st at 2:30pm',
        reminder: true,
      },
            {
        id: 2,
        text: 'Dentist Appointment',
        day: 'March 1st at 3:30pm',
        reminder: true,
      }
    ]
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
