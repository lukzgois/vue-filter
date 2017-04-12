<template>
  <div id="app">
    <basic-filter></basic-filter>
    <hr>
    <ajax-loader></ajax-loader>
    <hr>
    <load-items></load-items>
    <hr>
    <link-filter></link-filter>
    <hr>
    <multiple-selection></multiple-selection>
  </div>
</template>

<script>
import basicFilter from './examples/basic-filter/Index.vue'
import ajaxLoader from './examples/ajax-load/Index.vue'
import loadItems from './examples/load-items/Index.vue'
import linkFilter from './examples/link-filter/Index.vue'
import multipleSelection from './examples/multiple-selection/Index.vue'

export default {
  components: {
    basicFilter,
    ajaxLoader,
    loadItems,
    linkFilter,
    multipleSelection
  }
}
</script>

<style lang="sass">
  @import './css/vue-filter'
</style>
