<template>
  <div id="app">
    <basic-filter></basic-filter>
    <hr>
    <ajax-loader></ajax-loader>
    <hr>
    <load-items></load-items>
  </div>
</template>

<script>
import basicFilter from './examples/basic-filter/Index.vue'
import ajaxLoader from './examples/ajax-load/Index.vue'
import loadItems from './examples/load-items/Index.vue'

export default {
  components: {
    basicFilter,
    ajaxLoader,
    loadItems
  }
}
</script>

<style lang="sass">
  @import './css/vue-filter'
</style>
