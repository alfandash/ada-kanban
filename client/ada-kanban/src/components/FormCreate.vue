<template>
  <div id="form-task" v-if="showInput">
    <form class="form-horizontal" @submit.prevent="createtask('formNewTask')" ref="formNewTask">
      <fieldset>
        <div class="form-group">
          <label for="inputTitle" class="col-md-2 control-label">Title</label>
          <div class="col-md-5">
            <input class="form-control" id="inputTitle" placeholder="Title" type="text" v-model="task.title">
          </div>
        </div>
        <div class="form-group">
          <label for="inputAssing" class="col-md-2 control-label">Assign to</label>
          <div class="col-md-5">
            <input class="form-control" id="inputAssig" placeholder="Assign To" type="text" v-model="task.assignTo">
          </div>
        </div>
        <div class="form-group">
          <label for="textArea" class="col-md-2 control-label">Description</label>
          <div class="col-md-5">
            <textarea class="form-control" rows="3" id="textArea" v-model="task.desc"></textarea>
            <span class="help-block">You can type description here</span>
          </div>
        </div>
        <div class="form-group">
          <div class="col-md-5 col-md-offset-2">
            <button type="reset" class="btn btn-default" @click="cancelAction">Cancel</button>
            <button type="submit" class="btn btn-primary">Submit</button>
          </div>
        </div>
      </fieldset>
    </form>
  </div>
</template>

<script>

export default {
  props: ['showInput'],
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
    },
    cancelAction () {
      this.$emit('cancel')
    },
    resetFields () {
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
  }
}
</script>

<style>
#form-task {
  margin: 50px;
}
</style>
