<template>
  <div id="app">
    <h3>Multiple Selection </h3>
    <div class="filter-container">
      <dropdown
        title="Filter by label"
        text="Multiple"
        :items="items"
        :footer="footer"
        :header="header"
        :multiple="true"
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
  }
}
</script>
