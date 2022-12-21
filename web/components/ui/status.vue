<template>
  <span :class="styleStatus" class="flex items-center justify-center rounded-full px-2.5 py-0.5 text-xs mr-2.5">{{ status.label }}</span>
</template>

<script>
export default {
  props: {
    status: {
      type: Object,
      required: true
    }
  },
  computed: {
    styleStatus () {
      return this.status.value === 'DONE' ? 'text-green-800 bg-green-100' : 'text-red-800 bg-red-100'
    }
  }
}
</script>
