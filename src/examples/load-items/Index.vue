<template>
  <div id="app">
    <h3>Loading item when user scroll to bottom</h3>
    <div class="filter-container">
      <dropdown
        title="Filter by label"
        button-text="Labels"
        :items="items"
        :footer="footer"
        :searchable="true"
        :header="header"
        :bottom-message="bottomMessage"
        @filter-bottom-was-reached="loadItems"
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
      bottomMessage: null
    }
  },
  components: {
    Dropdown
  },
  methods: {
    loadItems () {
      this.bottomMessage = '<i class="fa fa-spinner fa-pulse fa-fw"></i>Carregando items...'
      setTimeout(() => {
        this.bottomMessage = null
        this.items.push({
          id: 6,
          text: 'Other State'
        }, {
          id: 7,
          text: 'Closed'
        })
      }, 1000)
    }
  }
}
</script>
