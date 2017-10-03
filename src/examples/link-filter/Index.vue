<template>
  <div id="app">
    <h3>Link instead of a button</small></h3>
    <div class="filter-container">
      <dropdown
        title="Filter by label"
        text="My Link"
        :render-as-link="true"
        :items="items"
        :footer="footer"
        :header="header"
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
  components: {
    Dropdown
  },
  methods: {
    filterWasClosed (value) {
      this.filterValue = value
      this.filterStatus = 'closed'
    },
    filterWasOpened () {
      this.filterStatus = 'opened'
    },
    filterWasFiltered (value) {
      this.filterInput = value
    },
    filterWasScrolled () {
      this.filterBottom = true
    }
  }
}
</script>
