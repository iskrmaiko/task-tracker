<template>
    <div v-show="showAddTask">
        <AddTask @add-task="addTask"/>
      </div>
      <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks" />
</template>

<script>
import Tasks from "../components/Tasks";
import AddTask from "../components/AddTask";
export default {
    name: 'Home',
    props: {
        showAddTask: Boolean,
    },
    components:{
        Tasks,
        AddTask
    },
    data() {
        return{
            tasks: [],
        }
    },
    methods: {
    async addTask(task) {
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
      })
      const data = await res.json()

      this.tasks = [...this.tasks, task]
    },
    async deleteTask(id) {
      if (confirm('Are you sure?')) {
        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE'
        });
        res.status === 200 ? (this.tasks = this.tasks.filter((task) => task.id !== id)) :
        alert('Error deleting task')
      }
    },
    async toggleReminder(id){
      const taskToToggle = await this.fetchTask(id)
      const updTask = { ...taskToToggle, reminder:
      !taskToToggle.reminder}

      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(updTask)
      })

      const data = await res.json()

      this.tasks =this.tasks.map((task) => task.id === id ? {...task, reminder: data.reminder } : task)
    },
    async fetchTasks() {
      // add a proxy using a vue.config.js to change the path (only on the dev server) from
      // const res = await fetch('http://localhost:5000/tasks') to the one below. restart server.
      const res = await fetch('api/tasks')
      const data = await res.json()
      return data
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`)
      const data = await res.json()
      return data
      //this will just return a single data
    }
  },
  async created(){
    // this.tasks=[{
    //   id: 1,
    //   title: 'Task 1',
    //   description: 'Task 1 description',
    //   status: 'pending',
    //   reminder: true
    // },
    // {
    //   id: 2,
    //   title: 'Task 2',
    //   description: 'Task 2 description',
    //   status: 'pending',
    //   reminder: true
    //   },
    // {
    //   id: 3,
    //   title: 'Task 3',
    //   description: 'Task 3 description',
    //   status: 'pending',
    //   reminder: false
    //   }]
    this.tasks = await this.fetchTasks()
  }
}
</script>