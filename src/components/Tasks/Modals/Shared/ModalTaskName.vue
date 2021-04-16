<template>
  <div class="row q-mb-sm">
    <q-input
      outlined
      :value="name"
      @input="$emit('update:name', $event)"
      label="Task name *"
      lazy-rules
      ref="name"
      class="col"
      :rules="[val => !!val || 'Please type something']" >
        <template v-slot:append>
          <q-icon
            v-if="name"
            name="close"
            @click="$emit('update:name', '')"
            class="cursor-pointer" />
        </template>
    </q-input>
  </div>
</template>

<script>
  export default {
    props: ['name']
  }
</script>

<style>

</style>

