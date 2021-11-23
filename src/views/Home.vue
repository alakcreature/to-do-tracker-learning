<template>
  <AddTask v-show="showAddTask" @add-task="addTask" />
  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script>
import Tasks from '../components/Tasks'
import AddTask from '../components/AddTask'
export default {
  name: 'Home',
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
    }
  },
  methods: {
    async addTask(task) {
      try{
        const res = await fetch('api/tasks', {
          method: 'POST',
          headers: {
            'Content-type': 'application/json',
          },
          body: JSON.stringify(task),
        })

        const data = await res.json()

        this.tasks = [...this.tasks, data]
      }
      catch(err){
        console.log(err);
      }
    },
    async deleteTask(id) {
      try{
        if (confirm('Are you sure?')) {
          const res = await fetch(`api/tasks/${id}`, {
            method: 'DELETE',
          })

          res.status === 200
            ? (this.tasks = this.tasks.filter((task) => task.id !== id))
            : alert('Error deleting task')
        }
      }
      catch(err){
        console.log(err);
      }
    },
    async toggleReminder(id) {
      // Not working
      // try{
      //   const taskToToggle = await this.fetchTask(id)
      //   const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder }

      //   const res = await fetch(`api/tasks/${id}`, {
      //     method: 'PUT',
      //     headers: {
      //       'Content-type': 'application/json',
      //     },
      //     body: JSON.stringify(updTask),
      //   })

      //   const data = await res.json()

      //   this.tasks = this.tasks.map((task) =>
      //     task.id === id ? { ...task, reminder: data.reminder } : task
      //   )
      // }
      // catch(err){
      //   console.log(err);
      // }
    },
    async fetchTasks() {
      try{
        const res = await fetch('api/tasks')

        const data = await res.json()

        return data
      }
      catch(err){
        console.log(err);
      }
    },
    async fetchTask(id) {
      try{
        const res = await fetch(`api/tasks/${id}`)

        const data = await res.json()

        return data
      }
      catch(err){
        console.log(err);
      }
    },
  },
  async created() {
    this.tasks = await this.fetchTasks()
  },
}
</script>
