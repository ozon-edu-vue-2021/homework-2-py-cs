<template>
  <div id="app">
    <Header :selectedItem="selectedItem.path" />
    <Node
      :tree="data"
      :path="`./${data.name}`"
    />
  </div>
</template>

<script>
import Node from './components/Node.vue'
import data from '../public/static/node_modules'
import Header from './components/Header.vue'

export default {
  name: 'App',
  components: {
    Node,
    Header,
  },
  data: () => ({
    data,
    selectedItem: {
      path: `./${data.name}`
    }
  }),
  mounted() {
    document.addEventListener('keydown', (e) => e.preventDefault());
  },
  provide() {
    return {
      selectedItem: this.selectedItem
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
