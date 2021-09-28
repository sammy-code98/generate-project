<template>
  <Generate />
</template>

<script>
import Generate from './components/Generate.vue'

export default {
  name: 'App',
  components: {
    Generate,
  }
}
</script>

<style>
html, body {
  margin: 0;
  height: 100%;
  background: #F9F9F9;
}
.container {
  padding-top: 20px;
}
</style>
