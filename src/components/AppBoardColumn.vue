<template>
  <app-drop @drop="moveTaskOrColumn">
    <app-drag class="column"
              :transferData="{
                type: 'column',
                fromColumnIndex: columnIndex
        }">
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
    </app-drag>
  </app-drop>
</template>

<script>
import AppColumnTask from '@/components/AppColumnTask'
import AppDrag from '@/components/AppDrag'
import AppDrop from '@/components/AppDrop'
import MovingTasksAndColumnsMixin from '@/mixins/MovingTasksAndColumnsMixin'

export default {
  components: {
    AppDrag,
    AppDrop,
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
