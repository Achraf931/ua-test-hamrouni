<template>
  <main class="py-10 px-5 container mx-auto flex-1">
    <SvgIcon :name="getDataType.icon" class="mx-auto w-10 h-10 mb-7" />
    <h1 class="font-bold text-center text-3xl mb-8">
      {{ getDataType.title }} a task
    </h1>
    <form class="flex flex-col gap-6 p-10 mx-auto max-w-sm bg-white rounded-xl shadow-md text-gray-700">
      <FormInput label="Title" name="title" :value="getDataTask.title" @input="form.title = $event" />
      <FormInputTextArea label="Describe it" name="description" :value="getDataTask.description" @input="form.description = $event" />
      <div class="flex gap-6">
        <FormInput
          class="w-1/2"
          label="Date"
          name="data"
          type="datetime-local"
          :value="getDataTask.date"
          @input="form.date = $event"
        />
        <FormSelect
          class="w-1/2"
          label="Status"
          name="status"
          :value="getDataTask.status"
          :options="filtersEdited"
          placeholder="Choose a status"
          @input="form.status = $event"
        />
      </div>
      <button type="button" class="py-2 px-4 transition-colors duration-100 ease-in-out rounded-md hover:bg-white hover:text-indigo-600 bg-indigo-600 text-white" @click="createOrEditTask">
        Submit
      </button>
      <NuxtLink to="/" class="text-center bg-gray-50 py-2 px-4 rounded-md hover:text-indigo-600">
        Cancel
      </NuxtLink>
      <small v-if="message" class="text-center text-red-600">{{ message }}</small>
    </form>
  </main>
</template>

<script>
import { TASK_STATUS } from '~/dewib/api/tasks'

export default {
  props: {
    formMode: {
      type: String,
      default: 'add'
    },
    task: {
      type: Object,
      default: null
    }
  },
  data () {
    return {
      filters: TASK_STATUS,
      message: '',
      form: {
        title: '',
        description: '',
        date: '',
        status: ''
      }
    }
  },
  computed: {
    getDataType () {
      return this.formMode === 'add' ? { icon: 'icons/plus-circle', title: 'Add' } : { icon: 'icons/pencil', title: 'Edit' }
    },
    getDataTask () {
      if (this.task) {
        this.setTaskData()
      }
      return this.form
    },
    filtersEdited () {
      const taskCloneStatus = JSON.parse(JSON.stringify(TASK_STATUS))
      taskCloneStatus.shift()
      return taskCloneStatus
    }
  },
  methods: {
    setTaskData () {
      this.form = { ...this.task, status: this.task?.status?.value }
    },
    async createOrEditTask () {
      if (this.form.title !== '' && this.form.description !== '' && this.form.date !== '' && this.form.status !== '') {
        return this.formMode === 'add' ? await this.$api.tasks.create(this.form) : await this.$api.tasks.update(this.form.id, this.form)
      } else {
        this.message = 'Please complete all fields'
      }
    }
  }
}
</script>
