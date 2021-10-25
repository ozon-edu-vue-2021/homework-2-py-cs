<template>
  <div class="node">
    <div @click="handleClick">
      {{ this.isSelected }} {{ icon }} {{ tree.name }}
      <div v-if="expanded">
        <Node
          v-for="item in tree.contents"
          :key="item.name"
          :tree="item"
          :path="`${path}/${item.name}`"
        />
      </div>
    </div>
  </div>
</template>

<script>
const ICONS = {
  file: "📝",
  directory: "📁",
  link: "🔗"
}
export default {
  name: 'Node',
  props: {
    tree: {
      type: Object,
      default: () => ({})
    },
    path: {
      type: String,
      default: ''
    }
  },
  data: () => ({
    expanded: false
  }),
  inject: ['selectedItem'],
  computed: {
    icon() {
      return ICONS[this.tree.type]
    },
    isDirectory() {
      return this.tree.type === 'directory'
    },
    isSelected() {
      return this.path === this.selectedItem.path ? '👉' : ''
    }
  },
  methods: {
    handleClick(event) {
      event.stopPropagation();
      this.selectedItem.path = this.path;
      if (this.isDirectory) {
        this.expanded = !this.expanded
      }
    }
  }
}
</script>

<style scoped>
.node {
  border-left: 1px solid #000;
  margin-left: 10px;
  padding: 5px 5px 5px 20px;
  text-align: left;
  cursor: pointer;
}
.path {
  position: absolute;
  top: 0;
}
</style>