<template>
 <div v-show="showaddtask" >
   <AddTask @add-task="addTask"/>
  </div>

  <Tasks 
  @toggle-reminder="togglereminder"
  @delete-task="deleteTask" :tasks="tasks"/>
</template>

<script>
import Tasks from '../components/Tasks.vue'
import AddTask from '../components/AddTask.vue'

export default {
name: 'Home',
props:{
showaddtask:Boolean,
},
components:{
    Tasks,
    AddTask
},
data(){
    return{
        tasks: [],
    }
},
methods:{
async addTask(task){
  const res = await fetch('api/tasks',{

    method:'POST',
    headers:{
      'Content-type':'application/json',
    },
    body: JSON.stringify(task),
  })
  const data = await res.json()
  this.tasks = [...this.tasks ,data]
},

async deleteTask(id){
  console.log('task',id)
  if(confirm('Are you sure?')){
    const res = await fetch(`api/tasks/${id}`,{
      method:'DELETE'
    })
    res.status ===200 ? (this.tasks = this.tasks.filter((task) => task.id !== id)) : alert('Error deleting task')
  

  }
},
async togglereminder(id) {
      const taskToToggle = await this.fetchtask(id)
      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder }
      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updTask),
      })
      const data = await res.json()
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      )
    },
// togglereminder(id){
//  this.tasks = this.tasks.map((task)=> task.id === id ? {...task, reminder: !task.reminder} :task
//  )
// },
async fetchtasks(){
  const res =await fetch('api/tasks')
  const data = await res.json()
  return data

},
async fetchtask(id){
  const res =await fetch(`api/tasks/${id}`)
  const data = await res.json()
  return data

},
  },
  async created(){
    this.tasks = await this.fetchtasks()
  }
}
</script>

<style>

</style>