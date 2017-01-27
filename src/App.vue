<template>
  <div id="app">
    <basic-filter></basic-filter>
    <hr>
    <ajax-loader></ajax-loader>
  </div>
</template>

<script>
import basicFilter from './examples/basic-filter/Index.vue'
import ajaxLoader from './examples/ajax-load/Index.vue'

export default {
  components: {
    basicFilter,
    ajaxLoader
  }
}
</script>
