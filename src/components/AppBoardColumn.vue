<template>
  <div class="column"
       draggable
       @drop="moveTaskOrColumn($event, column.tasks, columnIndex)"
       @dragover.prevent
       @dragenter.prevent
       @dragstart.self="pickupColumn($event, columnIndex)">
    <div class="flex items-center mb-2 font-bold">
      {{ column.name }}
    </div>
    <div class="list-reset">

      <app-column-task  v-for="(task, $taskIndex) in column.tasks"
                        :key="$taskIndex" :task="task"
                        :taskIndex="$taskIndex"
                        :board="board"
                        :column="column"
                        :columnIndex="columnIndex"/>

      <input type="text"
             class="block p-2 w-full bg-transparent"
             placeholder="+ Enter new task" @keyup.enter="createTask($event, column.tasks)">

    </div>
  </div>
</template>

<script>
import AppColumnTask from '@/components/AppColumnTask'
import MovingTasksAndColumnsMixin from '@/mixins/MovingTasksAndColumnsMixin'

export default {
  components: {
    AppColumnTask
  },
  mixins: [MovingTasksAndColumnsMixin],
  methods: {
    createTask (e, tasks) {
      this.$store.commit('CREATE_TASK', {
        tasks,
        name: e.target.value
      })
      e.target.value = ''
    },
    pickupColumn (e, fromColumnIndex) {
      e.dataTransfer.effectAllowed = 'move'
      e.dataTransfer.dropEffect = 'move'

      e.dataTransfer.setData('from-column-index', fromColumnIndex)
      e.dataTransfer.setData('type', 'column')
    }
  }
}
</script>

<style lang="css">
.column {
  @apply bg-grey-light p-2 mr-4 text-left shadow rounded;
  min-width: 350px;
}
</style>
