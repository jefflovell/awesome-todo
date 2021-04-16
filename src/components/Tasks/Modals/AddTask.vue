<template>
    <q-card>

      <modal-header>Add Task</modal-header>

      <q-form
        @submit="onSubmit"
        @reset="onReset"
        ref="newTask"
        :autofocus="true">
        <q-card-section class="q-pt-none">
          <modal-task-name
            :name.sync="taskToSubmit.name" />
          <modal-due-date
            :dueDate.sync="taskToSubmit.dueDate" 
            @clear="clearDueDate" />

          <div
            v-if="taskToSubmit.dueDate"
            class="row q-mb-sm">
            <q-input
              outlined
              v-model="taskToSubmit.dueTime"
              label="Due time"
              lazy-rules
              ref="dueTime" >
              <template v-slot:append>
                <q-icon
                  v-if="taskToSubmit.dueTime"
                  name="close"
                  @click="taskToSubmit.dueTime=''"
                  class="cursor-pointer" />
                <q-icon
                  v-else
                  name="access_time"
                  class="cursor-pointer">
                  <q-popup-proxy transition-show="scale" transition-hide="scale">
                    <q-time v-model="taskToSubmit.dueTime">
                      <div class="row items-center justify-end">
                        <q-btn v-close-popup label="Close" color="primary" flat />
                      </div>
                    </q-time>
                  </q-popup-proxy>
                </q-icon>
              </template>
            </q-input>
          </div>
        </q-card-section>
      
        <q-card-actions align="right">
          <div>
            <q-btn
            label="Cancel"
            v-close-popup
            color="grey-7"
            flat />
          <q-btn
            label="Reset"
            type="reset"
            color="grey-7"
            flat />
          <q-btn
            label="Save"
            type="submit"
            color="primary"
            flat />
          </div>
        </q-card-actions>
        <pre>{{taskToSubmit}}</pre>
      </q-form>
    </q-card>
</template>

<script>
  import { mapActions } from 'vuex'

  export default {
    data() {
      return {
        taskToSubmit: {
          name: '',
          dueDate: '',
          dueTime: '',
          completed: false
        }
      }
    },
    methods: {
      ...mapActions('tasks', ['addTask']),
      onSubmit() {
        this.$refs.name.validate()
        if ( !this.$refs.name.hasError ) {
          this.submitTask()
          this.$q.notify({
            icon: 'done',
            color: 'positive',
            message: 'Task added'
          })

        }
      },
      submitTask() {
        console.log('submitTask')
        this.addTask(this.taskToSubmit)
        this.$emit('close')
      },
      onReset() {
        this.taskToSubmit.name=null
        this.taskToSubmit.dueDate=null
        this.taskToSubmit.dueTime=null

      },
      clearDueDate() {
        this.taskToSubmit.dueDate=''
        this.taskToSubmit.dueTime=''
      }
      
    },
    components: {
      'modal-header': require('components/Tasks/Modals/Shared/ModalHeader.vue').default,
      'modal-task-name': require('components/Tasks/Modals/Shared/ModalTaskName.vue').default,
      'modal-due-date': require('components/Tasks/Modals/Shared/ModalDueDate.vue').default
    }
  }
  
</script>

<style>

</style>