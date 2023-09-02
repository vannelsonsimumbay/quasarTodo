<template>
  <q-page class="bg-white-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input class="col" @keyup.enter="addTask" square bg-color="white" filled v-model="newTask" dense>
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input>
    </div>
    <q-list clas="bg-shite  " separator bordered>
      <q-item v-ripple v-for="(task, index) in tasks" :key="task.title" @click="task.done = !task.done" clickable
        :class="{ 'done bg-blue-1': task.done }">
        <q-item-section avatar>
          <q-checkbox v-model="task.done" class="no-pointer-events" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn @click.stop="deleteTask(index)" size="sm" round color="negative" icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
    <div class="no-taks absolute-center" v-if="!tasks.length">
      <q-icon name="check" size="100px" color="primary"></q-icon>
      <div class="text-h5 text-primary text-center">
        No tasks
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from 'vue';
import { useQuasar, Notify } from 'quasar'

const $q = useQuasar()

const tasks = ref([])
const newTask = ref('')

const addTask = () => {
  tasks.value.push({ title: newTask.value, done: false })
  newTask.value = '';
}

const deleteTask = (index) => {
  $q.dialog({
    title: 'Confirm',
    message: 'Would you like to turn on the wifi?',
    cancel: true,
    persistent: true
  }).onOk(() => {
    tasks.value.splice(index, 1)
    $q.notify('Task deleted!')
  })
}
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}

.no-taks {
  opacity: 0.5;
}
</style>