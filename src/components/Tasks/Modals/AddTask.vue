<template>
    <q-card>

      <q-card-section class="row">
              <div class="text-h6">Add Task</div>
              <q-space />
              <q-btn
                v-close-popup
                flat
                round
                icon="close" />
      </q-card-section>

      <q-form
        @submit="onSubmit"
        @reset="onReset"
        ref="newTask"
        :autofocus="true">
        <q-card-section class="q-pt-none">
          <div class="row q-mb-sm">
            <q-input
              outlined
              v-model="taskToSubmit.name"
              label="Task name *"
              lazy-rules
              ref="name"
              class="col"
              :rules="[val => !!val || 'Please type something']" >
                <template v-slot:append>
                  <q-icon
                    v-if="taskToSubmit.name"
                    name="close"
                    @click="taskToSubmit.name=''"
                    class="cursor-pointer" />
                </template>
            </q-input>
          </div>

          <div class="row q-mb-sm">
            <q-input
              outlined
              v-model="taskToSubmit.dueDate"
              label="Due date"
              lazy-rules
              ref="dueDate" >
              <template v-slot:append>
                <q-icon
                  v-if="taskToSubmit.dueDate"
                  name="close"
                  @click="clearDueDate"
                  class="cursor-pointer" />
                <q-icon
                  v-else
                  name="event"
                  class="cursor-pointer" >
                  <q-popup-proxy ref="qDateProxy" transition-show="scale" transition-hide="scale">
                    <q-date v-model="taskToSubmit.dueDate">
                      <div class="row items-center justify-end">
                        <q-btn v-close-popup label="Close" color="primary" flat />
                      </div>
                    </q-date>
                  </q-popup-proxy>
                </q-icon>
              </template>
            </q-input>
          </div>

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
      
    }
  }
  
</script>

<style>

</style>