<template>
  <div id="app">
    <h3>Basic Filter <small>(filter basics functions)</small></h3>
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
        @filter-bottom-was-reached="filterWasScrolled"
      ></dropdown>

      <div style="margin-left: 180px; line-height: 140%">
        Filter is: {{ filterStatus }} <br>
        Selected Value: {{ filterValue }} <br>
        Filter Input Value: {{ filterInput }} <br>
        Filter was scrolled to bottom: {{ filterBottom }} <br>
      </div>
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

<style lang="sass">
  @import '../../css/vue-filter'
</style>
