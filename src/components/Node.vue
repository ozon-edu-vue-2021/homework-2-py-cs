<template>
  <div class="node">
    <div @click="handleClick">
      <div ref="node" class="node-name">
        {{ this.isSelected ? '👉' : '' }} {{ icon }} {{ tree.name }}
      </div>
      <div v-if="expanded">
        <Node
          ref="children"
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
  mounted() {
    this.updateFocus()
  },
  updated() {
    this.updateFocus()
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
      return this.path === this.selectedItem.path
    },
    tabIndex() {
      return Number(this.isSelected);
    },
    index() {
      return this.$parent?.tree?.contents?.indexOf(this.tree);
    },
    kbNext() {
      return (this.isDirectory && this.expanded)
        ? this.$refs.children[0].path
        : this.$parent.$refs.children[this.index + 1]?.path
    },
    kbPrev() {
      return this.$parent.$refs.children[this.index - 1]?.path;
    }
  },
  methods: {
    updateFocus() {
      const node = this.$refs.node;
      if (this.isSelected) {
        node.tabIndex = 1;
        node.focus();
        node.addEventListener('keyup', this.handleKey)
      } else {
        delete node.tabIndex;
        node.removeEventListener('keyup', this.handleKey)
      }
    },
    handleClick(event) {
      event.stopPropagation();
      this.selectedItem.path = this.path;
      if (this.isDirectory) {
        this.expanded = !this.expanded
      }
    },
    handleKey(event) {
      const { key } = event;
      switch (String(key)) {
        case "ArrowRight":
          if (this.isDirectory) {
            this.expanded = true;
          }
          break;
        case "ArrowLeft":
          this.$parent.expanded = false;
          this.selectedItem.path = this.$parent.path;
          break;
        case "ArrowDown":
            this.selectedItem.path = this.kbNext || this.path;
          break;
        case "ArrowUp":
          this.selectedItem.path = this.kbPrev || this.path;
          break;
        default:
          break;
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
.node-name:focus {
  outline: none;
}
.path {
  position: absolute;
  top: 0;
}
</style>