<template>
  <div id="app">
    <h3>Footer items <small>(log the footer item clicked on console)</small></h3>
    <div class="filter-container">
      <dropdown
        title="Filter by label"
        text="Labels"
        :items="items"
        :header="header"
        :footer="footer"
        @footer-was-clicked="handleFooterClick"
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
      items,
      header,
      footer,
      filterValue: null,
      filterStatus: 'closed',
      filterBottom: false,
      filterInput: ''
    }
  },
  methods: {
    handleFooterClick (item) {
      console.log(item)
    }
  },
  components: {
    Dropdown
  }
}
</script>
