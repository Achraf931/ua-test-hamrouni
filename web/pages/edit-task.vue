<template>
  <Form form-mode="edit" :task="task" />
</template>

<script>
export default {
  data () {
    return {
      task: {}
    }
  },
  async fetch () {
    const [err, task] = await this.$api.tasks.findOne(this.$route.params.id)

    if (err) {
      throw new Error(err)
    }

    this.task = task
  }
}
</script>
