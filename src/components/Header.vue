<template>
  <div class="header">
    <div class="path">
      {{ selectedItem }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'Header',
  props: {
    selectedItem: {
      type: String,
      default: ''
    }
  }
}
</script>

<style scoped>
.header {
  display: block;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  padding: 20px 0;
  background-color: #fff;
}
.path {
  margin: 0 auto;
  line-height: 24px;
  height: 20px;
  max-width: 1200px;
  border: 1px solid teal;
  padding: 4px 8px;
  text-align: left;
  overflow: hidden;  
}
</style>