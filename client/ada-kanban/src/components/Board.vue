<template>
  <div id="board">
    <div class="row">
      <div class="col-md-12">
        <div class="col-md-12">
          <h1>ADA: Kanban</h1>
        </div>
      </div>
      <div class="col-md-12">
        <FormCreate @submitTask="submitTask"
      :showInput="inputForm"
      @cancel="inputForm = false, tasksBoard = true" :progress="progress"></FormCreate>
      </div>  
    </div>
    <div class="row" v-if="tasksBoard">
      <div class="col-md-12">
        <div class="col-md-12">
          <p><button class="btn btn-primary" @click="inputForm = true, tasksBoard = false"> Input task </button></p>
        </div>
        <div class="col-md-3">
          <Task status="stat_0" :tasks="stat_0" @nextAction="nextAction" @backAction="backAction" @deleteAction="deleteAction"></Task>
        </div>
        <div class="col-md-3">
          <Task status="stat_1" :tasks="stat_1" @nextAction="nextAction" @backAction="backAction" @deleteAction="deleteAction"></Task>
        </div>
        <div class="col-md-3">
          <Task status="stat_2" :tasks="stat_2" @nextAction="nextAction" @backAction="backAction" @deleteAction="deleteAction"></Task>
        </div>
        <div class="col-md-3">
          <Task status="stat_3" :tasks="stat_3" @nextAction="nextAction" @backAction="backAction" @deleteAction="deleteAction"></Task>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import firebase from 'firebase'
import FormCreate from '@/components/FormCreate'
import Task from '@/components/Task'

const config = {
  databaseURL: 'https://ada-firebase.firebaseio.com',
  projectId: 'ada-firebase'
}

firebase.initializeApp(config)
var db = firebase.database()
const tasksRef = db.ref('tasks')
export default {
  components: {
    FormCreate,
    Task
  },
  firebase: {
    tasks: tasksRef
  },
  data () {
    return {
      inputForm: false,
      tasksBoard: true,
      progress: false
    }
  },
  methods: {
    submitTask (data) {
      this.tasksBoard = true
      this.inputForm = false
      console.log(data)
      tasksRef.push(data)
      .then((response) => {
        this.progress = true
      })
    },
    nextAction (data) {
      let status = data.stat + 1
      tasksRef.child(data.key)
      .child('status')
      .set(status)
    },
    backAction (data) {
      let status = data.stat - 1
      tasksRef.child(data.key)
        .child('status')
        .set(status)
    },
    deleteAction (data) {
      tasksRef.child(data.key).remove()
    }
  },
  computed: {
    stat_0 () {
      return this.tasks.filter((task) => task.status === 0)
    },
    stat_1 () {
      return this.tasks.filter((task) => task.status === 1)
    },
    stat_2 () {
      return this.tasks.filter((task) => task.status === 2)
    },
    stat_3 () {
      return this.tasks.filter((task) => task.status === 3)
    }
  }
}
</script>


<style>
#board {
  padding: 10px;
}

</style>
