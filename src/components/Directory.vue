<template>
  <div @click="toggle">
    📁 {{ node.name }}
    <div v-if="expanded">
      <Node
        v-for="item in node.contents"
        :key="item.name"
        :node="item"
        :path="`${path}/${item.name}`"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Directory',
  props: () => ({
    node: {
      type: Object,
      default: {}
    },
    path: {
      type: String,
      default: ''
    }
  }),
  data: () => ({
    expanded: false
  }),
  methods: {
  toggle(event) {
    this.expanded = !this.expanded
    event.stopPropagation();
    }
  }
}
</script>

<style>

</style>