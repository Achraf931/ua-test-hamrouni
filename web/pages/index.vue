<template>
  <main class="py-10 px-5 container mx-auto flex-1">
    <div class="flex py-5 items-center justify-between mb-8 border-b border-solid border-gray-200">
      <h1 class="text-lg text-gray-700">
        My Tasks
      </h1>
      <FormSelect class="w-40" name="Filters" :value="filterSelected" :options="filters" @input="filterSelected = $event" />
    </div>
    <p v-if="$fetchState.pending">
      Fetching tasks...
    </p>
    <p v-else-if="$fetchState.error" class="text-red-700 text-2xl py-5">
      Unable to fetch tasks.
    </p>
    <div v-else>
      <ul class="bg-white rounded-xl shadow-md">
        <li v-for="task in getTasks" :key="task.id" class="text-sm text-gray-500 px-6 py-4 gap-2 border-b border-[#E5E7EB] w-full first:rounded-t-xl last:rounded-b-xl">
          <div class="flex items-center">
            <UiStatus :status="task.status" />
            <p class="text-indigo-600">
              {{ task.title }}
            </p>
            <div class="flex gap-2 ml-auto">
              <NuxtLink :to="{ name: 'id', params: { id: task.id } }">
                <SvgIcon name="icons/pencil" class="p-1 w-5 h-5 bg-indigo-600 text-white rounded" />
              </NuxtLink>
              <SvgIcon @click="removeTask(task.id)" name="icons/trash" class="p-1 w-5 h-5 bg-red-600 text-white rounded cursor-pointer" />
            </div>
          </div>
          <div class="flex flex-col md:flex-row items-baseline justify-between gap-2 my-2.5">
            <p v-html="task.description" />
            <div class="flex items-center gap-2">
              <SvgIcon name="icons/calendar" class="w-4 h-4 text-gray-400 rounded" />
              <p class="text-xs md:text-sm">
                {{ new Date(task.createdAt).toLocaleDateString('en-US', { weekday: 'short', year: 'numeric', month: 'long', day: '2-digit' }) }}
              </p>
            </div>
          </div>
          <div class="flex flex-col md:flex-row gap-2 text-xs">
            <p>{{ `Created at ${new Date(task.createdAt).toLocaleDateString('en-US', { weekday: 'short', year: 'numeric', month: 'long', day: '2-digit', hour: 'numeric', hourCycle: 'h24' })}` }}</p>
            <p class="font-semibold">
              {{ `Updated at ${new Date(task.updatedAt).toLocaleDateString('en-US', { weekday: 'short', year: 'numeric', month: 'long', day: '2-digit', hour: 'numeric', hourCycle: 'h24' })}` }}
            </p>
          </div>
        </li>
      </ul>
    </div>
  </main>
</template>

<script>
import { TASK_STATUS } from '~/dewib/api/tasks'

export default {
  data () {
    return {
      tasks: [],
      filters: TASK_STATUS,
      filterSelected: 'ALL'
    }
  },
  async fetch () {
    await this.fetchAllTasks()
  },
  computed: {
    getTasks () {
      return this.filterSelected === 'ALL' ? this.tasksSorted(this.tasks) : this.tasksSorted(this.tasks).filter(e => e.status.value === this.filterSelected)
    }
  },
  methods: {
    async fetchAllTasks () {
      const [err, tasks] = await this.$api.tasks.findAll()

      if (err) {
        throw new Error(err)
      }

      this.tasks = tasks
    },
    tasksSorted (tasks) {
      return tasks.sort((a, b) => a.date.localeCompare(b.date) || b.createdAt - a.createdAt)
    },
    async removeTask (id) {
      await this.$api.tasks.remove(id)
      await this.fetchAllTasks()
    }
  }
}
</script>
