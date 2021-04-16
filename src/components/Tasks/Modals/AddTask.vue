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
            :name.sync="taskToSubmit.name" 
            ref="modalTaskName" />
          <modal-due-date
            :dueDate.sync="taskToSubmit.dueDate" 
            @clear="clearDueDate" />
          <modal-due-time
            :dueTime.sync="taskToSubmit.dueTime"
            v-if="taskToSubmit.dueDate" />
        </q-card-section>
      
        <modal-buttons />

        
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
        this.$refs.modalTaskName.$refs.name.validate()
        if ( !this.$refs.modalTaskName.$refs.name.hasError ) {
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
      'modal-due-date': require('components/Tasks/Modals/Shared/ModalDueDate.vue').default,
      'modal-due-time': require('components/Tasks/Modals/Shared/ModalDueTime.vue').default,
      'modal-buttons': require('components/Tasks/Modals/Shared/ModalButtons.vue').default
    }
  }
  
</script>

<style>

</style>