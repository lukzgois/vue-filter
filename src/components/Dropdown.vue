<template>
  <div class="filter" :class="isOpen ? 'dropdown-open' : ''" ref="dropdown">
    <button class="dropdown-menu-toggle" @click="isOpen = !isOpen">
      {{ buttonText }}
      <i class="fa fa-angle-down" aria-hidden="true"></i>
    </button>

    <div class="dropdown-menu">
      <div class="dropdown-title">
        {{ title }}
        <a href="#" class="dropdown-menu-close" @click.prevent="isOpen = false">
          <i class="fa fa-times" aria-hidden="true"></i>
        </a>
      </div>

      <div class="dropdown-input-container" v-if="searchable">
        <input type="text" class="dropdown-input" v-model="filterInputValue" @input="userIsTyping" />
        <i class="fa fa-search dropdown-input-icon" aria-hidden="true"></i>
      </div>

      <div class="dropdown-content">
        <ul class="dropdown-header-items" v-if="header">
          <li v-for="item in header">
            <a
              href="#"
              @click.prevent="handleHeaderClick(item, $event)"
            >{{ item.text }}</a>
          </li>
        </ul>
        <ul>
          <li v-for="item in items">
            <a
              href="#"
              @click.prevent="handleItemClick(item, $event)"
            >{{ item.text }}</a>
          </li>
        </ul>
      </div>

      <div class="dropdown-footer" v-if="footer">
        <ul>
          <li v-for="item in footer">
            <a href="#">{{ item.text }}</a>
          </li>
        </ul>
      </div>

      <div class="dropdown-loading" :class="isLoading ? 'show' : ''">
        <i class="fa fa-spinner fa-pulse fa-3x fa-fw"></i>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  props: ['button-text', 'title', 'items', 'footer', 'searchable', 'header', 'is-loading'],
  data () {
    return {
      isOpen: false,
      selectedValue: null,
      filterInputValue: ''
    }
  },
  created () {
    window.addEventListener('click', (e) => {
      if (!this.isFilterElement(e.target)) {
        this.isOpen = false
      }
    })
  },
  methods: {
    isFilterElement (e) {
      if (!e.parentNode || e.parentNode.toString() === '[object HTMLDocument]') {
        return false
      }

      if (e.parentNode === this.$refs.dropdown) {
        return true
      }

      return this.isFilterElement(e.parentNode)
    },
    handleHeaderClick (item, e) {
      const value = item.id ? item.id : item.value

      this.selectedValue = value
      this.isOpen = false

      let oldSelection = window.document.querySelector('.filter li a.is-selected')
      if (oldSelection) {
        oldSelection.classList.remove('is-selected')
      }

      e.target.classList.add('is-selected')
    },
    handleItemClick (item, e) {
      const value = item.id ? item.id : item.value

      this.selectedValue = value
      this.isOpen = false

      let oldSelection = window.document.querySelector('.filter li a.is-selected')
      if (oldSelection) {
        oldSelection.classList.remove('is-selected')
      }

      e.target.classList.add('is-selected')
    },
    userIsTyping (e) {
      this.$emit('filter-input', e.target.value)
    }
  },
  watch: {
    isOpen (val, oldVal) {
      if (!val) {
        this.$emit('filter-closed', this.selectedValue)
        return
      }

      this.$emit('filter-opened')
    }
  }
}
</script>

<style lang="sass">
  .dropdown-menu-toggle
    background: #fff
    color: #626262
    padding: 6px 10px
    border: 1px solid #c4c4c4
    border-radius: 3px
    font-size: 15px
    width: 150px
    text-align: left
    position: relative
    cursor: pointer
    display: flex
    justify-content: space-between

  .dropdown-menu
    background: #fff
    margin-top: 5px
    width: 280px
    padding: 2px 8px
    border-radius: 4px
    color: #555
    position: absolute
    display: none
    z-index: 10

  .dropdown-open .dropdown-menu
    display: block

  .dropdown-title
    color: #5c5c5c
    text-align: center
    white-space: nowrap
    font-weight: 600
    padding: 8px 0
    margin-right: 3px
    border-bottom: 1px solid rgba(0,0,0,0.1)

  .dropdown-menu-close
    position: absolute
    right: 23px;
    color: #ababab

  .dropdown-input-container
    margin: 10px 15px 10px 0
    position: relative

    .fa
      font-size: .9em
      position: absolute
      top: 9px
      right: 0
      color: #ababab

  .dropdown-input
    width: 100%
    padding: 0 5px
    color: #555
    border-radius: 2px
    line-height: 30px
    border: 1px solid rgba(0,0,0,0.1)

  .dropdown-content, .dropdown-footer
    max-height: 210px
    overflow-y: scroll
    margin: 10px 0

    ul:first-child
      padding-top: 0

    ul:last-child
      padding-bottom: 0

    ul
      list-style: none
      padding: 10px 0
      margin: 0

      li.divider
        border-bottom: 1px solid rgba(0,0,0,0.1)
        margin: 0 0 5px 0

      li a
        padding: 5px 30px
        display: block
        text-decoration: none
        color: #555
        position: relative

      li a:hover
        background: #fbfbfb

      li a.is-selected::before
        content: "\f00c"
        font: normal normal normal 14px/1 FontAwesome
        margin-right: 5px
        position: absolute
        top: 8px
        left: 10px

    ul.dropdown-header-items
      border-bottom: 1px solid rgba(0,0,0,0.1)
      margin-right: 5px

  .dropdown-footer
    overflow-y: auto
    border-top: 1px solid rgba(0,0,0,0.1)
    margin-top: 10px
    font-size: 0.9em

  .dropdown-loading
    position: absolute
    top: 0
    bottom: 0
    left: 0
    right: 0
    justify-content: center
    align-items: center
    background: rgba(255, 255, 255, 0.7)
    display: none

  .dropdown-loading.show
    display: flex

</style>
