<template>
  <div class="m-10 container mx-auto">
    <ul class="list-disc">
      <li>
        <div class="inline-flex gap-x-2">
          <a href="https://github.com/nuxt-community/svg-sprite-module" target="_blank">Use @nuxtjs/svg-sprite for icons</a>
          <!-- You should use SvgIcon to use the icons -->
          <SvgIcon name="icons/pencil" class="p-1 w-5 h-5 bg-indigo-600 text-white rounded" />
        </div>
      </li>
      <li>Use the power of <b>tailwindcss</b> for the css</b></li>
      <li><b>TASK_STATUS</b> could be helpful. <b>~/dewib/api/tasks/index.js</b></li>
      <li>All requests to the API will be located in the <b>`~/dewib/api/tasks/index`</b> file</li>
      <li>Check <b>TASKS_FIELDS</b> for the mapping to be used from your page <b>~/dewib/api/tasks/index.js</b></li>
      <li>Form components could be helpful <b>~/components/form</b></li>
    </ul>
    <p v-if="$fetchState.pending">
      Fetching tasks...
    </p>
    <p v-else-if="$fetchState.error" class="text-red-700 text-2xl py-5">
      Unable to fetch tasks.
    </p>
    <pre v-else>{{ tasks }}</pre>
  </div>
</template>

<script>
export default {
  data () {
    return {
      tasks: []
    }
  },
  async fetch () {
    const [err, tasks] = await this.$api.tasks.findAll()

    if (err) {
      throw new Error(err)
    }

    this.tasks = tasks
  }
}
</script>
