<template>
<transition name="fade" v-if="showInput">
  <div id="form-task" v-if="showInput">
      <form class="form-horizontal" @submit.prevent="createtask('formNewTask')" ref="formNewTask">
        <fieldset>
          <div class="form-group">
            <label for="inputTitle" class="col-md-1 control-label">Title</label>
            <div class="col-md-4">
              <input class="form-control" id="inputTitle" placeholder="Title" type="text" v-model="task.title">
            </div>
          </div>
          <div class="form-group">
            <label for="inputAssing" class="col-md-1 control-label">Assign to</label>
            <div class="col-md-4">
              <input class="form-control" id="inputAssig" placeholder="Assign To" type="text" v-model="task.assignTo">
            </div>
          </div>
          <div class="form-group">
            <label for="textArea" class="col-md-1 control-label">Description</label>
            <div class="col-md-4">
              <textarea class="form-control" rows="2" id="textArea" v-model="task.desc"></textarea>
              <span class="help-block">You can type description here</span>
            </div>
          </div>
          <div class="form-group">
            <div class="col-md-4 col-md-offset-1">
              <button type="reset" class="btn btn-default" @click="cancelAction">Cancel</button>
              <button type="submit" class="btn btn-primary">Submit</button>
            </div>
          </div>
        </fieldset>
      </form>
    </div>
  </transition>
</template>

<script>

export default {
  props: ['showInput', 'progress'],
  data () {
    return {
      task: {
        title: null,
        desc: null,
        assignTo: null,
        status: 0
      }
    }
  },
  methods: {
    createtask (name) {
      this.$emit('submitTask', this.task)
      this.resetFields()
    },
    cancelAction () {
      this.$emit('cancel')
      this.resetFields()
    },
    resetFields () {
      // console.log(this.$task)
      this.task.title = null
      this.task.desc = null
      this.task.assignTo = null
      this.task.status = 0
    }
  },
  computed: {
    showForm () {
      return this.show
    }
  },
  watch: {
    progress: function (newvalue) {
      this.resetFields()
    }
  }
}
</script>

<style>
#form-task {
  margin: 10px;
}
.fade-enter-active {
  transition: opacity 0.5s
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0
}
</style>
