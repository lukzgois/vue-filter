<template>
  <div id="app">
    <div class="filter-container">
      <dropdown
        title="Filter by label"
        button-text="Labels"
        :items="items"
        :footer="footer"
        :searchable="true"
        :header="header"
        @filter-closed="filterWasClosed"
        @filter-opened="filterWasOpened"
        @filter-input="filterWasFiltered"
      ></dropdown>

      <div style="margin-left: 180px; line-height: 140%">
        Filter is: {{ filterStatus }} <br>
        Selected Value: {{ filterValue }} <br>
        Filter Input Value: {{ filterInput }} <br>
      </div>
    </div>
  </div>
</template>

<script>
import Dropdown from './components/Dropdown'
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
    }
  }
}
</script>

<style lang="sass">
  @import './css/vue-filter'
</style>
