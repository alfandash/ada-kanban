<template>
  <div id="board">
    <button class="btn btn-primary" @click="inputForm = true, tasksBoard = false"> Input task </button>
    <div class="row">
      <FormCreate @submitTask="submitTask"
      :showInput="inputForm"
      @cancel="inputForm = false"></FormCreate>  
    </div>
    <div class="row" v-if="tasksBoard">
      <div class="col-md-12">
        <h1>ADA: Kanban</h1>
        <h2>{{tasks}} </h2>
        <div class="col-md-3">
          <Task status="stat_0" :tasks="stat_0" @nextAction="nextAction" @backAction="backAction"></Task>
        </div>
        <div class="col-md-3">
          <Task status="stat_1" :tasks="stat_1" @nextAction="nextAction" @backAction="backAction"></Task>
        </div>
        <div class="col-md-3">
          <Task status="stat_2" :tasks="stat_2" @nextAction="nextAction" @backAction="backAction"></Task>
        </div>
        <div class="col-md-3">
          <Task status="stat_3" :tasks="stat_3" @nextAction="nextAction" @backAction="backAction"></Task>
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
      tasksBoard: true
    }
  },
  methods: {
    submitTask (data) {
      this.tasksBoard = true
      this.inputForm = false
      console.log(data)
      tasksRef.push(data)
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
  padding: 20px;
}

</style>
