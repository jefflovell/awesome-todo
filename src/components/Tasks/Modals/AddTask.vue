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
              :rules="[val => !!val || 'Please type something']" />
          </div>

          <div class="row q-mb-sm">
            <q-input
              outlined
              v-model="taskToSubmit.dueDate"
              label="Due date"
              lazy-rules
              ref="dueDate" >
              <template v-slot:append>
                <q-icon name="event" class="cursor-pointer">
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

          <div class="row q-mb-sm">
            <q-input
              outlined
              v-model="taskToSubmit.dueTime"
              label="Due time"
              lazy-rules
              ref="dueTime" >
              <template v-slot:append>
                <q-icon name="access_time" class="cursor-pointer">
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
          <div class="row justify-end">
            <q-checkbox
              v-model="addMultiple"
              @click="addMultiple = !addMultiple"
              align="right"
              label="Add multiple tasks?"
              left-label />
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
            flat
            v-close-popup="!addMultiple" />
          </div>
        </q-card-actions>
        
      </q-form>
    </q-card>
</template>

<script>
  export default {
    data() {
      return {
        addMultiple: false,
        taskToSubmit: {
          name: '',
          dueDate: '',
          dueTime: '',
          completed: false
        }
      }
    },
    methods: {
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
        this.onReset()
      },
      onReset() {
        this.taskToSubmit.name = ''
        this.taskToSubmit.dueDate = ''
        this.taskToSubmit.dueTime = ''

        this.$refs.name.resetValidation()
        this.$refs.dueDate.resetValidation()
        this.$refs.dueTime.resetValidation()
      }
      
    }
  }
  
</script>

<style>

</style>