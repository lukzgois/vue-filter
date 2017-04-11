<template>
  <div id="app">
    <h3>Ajax Loader <small>(simulating an ajax request to server)</small></h3>
    <div class="filter-container">
      <dropdown
        title="Filter by label"
        text="Labels"
        :items="items"
        :footer="footer"
        :is-loading="loading"
        :searchable="true"
        :header="header"
        @filter-opened="filterWasOpened"
        @filter-closed="filterWasClosed"
      ></dropdown>
    </div>
  </div>
</template>

<script>
import Dropdown from '../../components/Dropdown'
import {items, header, footer} from './filterConfig'

export default {
  name: 'app',
  data () {
    return {
      items: {},
      header,
      footer,
      loading: true,
      timeout: null
    }
  },
  components: {
    Dropdown
  },
  methods: {
    filterWasOpened () {
      this.timeout = setTimeout(() => {
        this.items = items
        this.loading = false
      }, 500)
    },
    filterWasClosed () {
      clearTimeout(this.timeout)
      this.loading = true
      this.items = {}
    }
  }
}
</script>
