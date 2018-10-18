<template>
  <datatable />
</template>

<script>
import datatable from './components/datatable.vue'

export default {
  name: 'app',
  components: {
    datatable
  }
}

</script>

<style>
body {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  margin-top: 60px;
}
</style>
