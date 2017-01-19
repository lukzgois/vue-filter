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

export default {
  name: 'app',
  data () {
    return {
      items: [{
        id: 1,
        text: 'Ready to start'
      }, {
        id: 2,
        text: 'In Progress'
      }, {
        id: 3,
        text: 'Evaluating'
      }, {
        id: 4,
        text: 'Ready to Test'
      }, {
        id: 5,
        text: 'Awaiting deploy'
      }, {
        id: 5,
        text: 'Other State'
      }, {
        id: 5,
        text: 'Closed'
      }],
      footer: [{
        text: 'Create new user'
      }, {
        text: 'Manage users'
      }],
      header: [{
        id: 'any',
        text: 'Any Label'
      }, {
        id: 'none',
        text: 'No Label'
      }],
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

<style>
  @import 'https://fonts.googleapis.com/css?family=Source+Sans+Pro';

  body {
    background: #f1f1f1;
    font-family: "Source Sans Pro"
  }

  .filter-container {
    display: flex;
  }

  .filter {
    margin-left: 10px
  }

  .filter:first-child {
    margin-left: 0
  }
</style>
