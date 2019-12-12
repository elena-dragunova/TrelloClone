<template>
  <app-drop @drop="moveTaskOrColumn">
    <app-drag class="task"
              :transferData="{
                type: 'task',
                fromColumnIndex: columnIndex,
                fromTaskIndex: taskIndex
        }"
              @click="goToTask(task)">

      <span class="w-full flex-no-shrink font-bold">
        {{ task.name }}
      </span>
      <p v-if="task.description"
         class="w-full flex-no-shrink mt-1 text-sm">
        {{ task.description }}
      </p>
    </app-drag>
  </app-drop>
</template>

<script>
import AppDrag from '@/components/AppDrag'
import AppDrop from '@/components/AppDrop'
import MovingTasksAndColumnsMixin from '@/mixins/MovingTasksAndColumnsMixin'

export default {
  components: {
    AppDrag,
    AppDrop
  },
  props: {
    task: {
      type: Object,
      required: true
    },
    taskIndex: {
      type: Number,
      required: true
    }
  },
  mixins: [MovingTasksAndColumnsMixin],
  methods: {
    goToTask (task) {
      this.$router.push({ name: 'task', params: { id: task.id } })
    }
  }
}
</script>

<style lang="css">
.task {
  @apply flex items-center flex-wrap shadow mb-2 py-2 px-2 rounded bg-white text-grey-darkest no-underline;
}
</style>
