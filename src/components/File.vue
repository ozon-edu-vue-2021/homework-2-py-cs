<template>
  <div v-if="isFile" @click="showName">
    📝 {{ node.name }}
  </div>
  <div v-else @click="showName">
    🔗 {{ node.name }}
  </div>
</template>

<script>
export default {
  name: 'File',
  props: () => ({
    node: {
      type: Object,
      default: {}
    }
  }),
  computed: {
    isFile() {
      return this.node.type === 'file'
    }
  }
}
</script>

<style>

</style>