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
        <ul class="dropdown-items">
          <li v-for="item in items">
            <a
              href="#"
              @click.prevent="handleItemClick(item, $event)"
            >{{ item.text }}</a>
          </li>
          <!-- Bottom message -->
          <li class="bottom-message" v-show="bottomMessage" v-html="bottomMessage"></li>
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
import debounce from '../utils/debounce'

export default {
  props: {
    buttonText: {
      type: String,
      default: 'Filter'
    },
    title: {
      type: String,
      default: 'Filter'
    },
    header: {
      type: Array,
      default: []
    },
    items: {
      type: Array,
      default: []
    },
    footer: {
      type: Array,
      default: []
    },
    searchable: {
      type: Boolean,
      default: false
    },
    isLoading: {
      type: Boolean,
      default: false
    },
    bottomMessage: {
      type: String,
      default: ''
    }
  },
  data () {
    return {
      isOpen: false,
      selectedValue: null,
      filterInputValue: ''
    }
  },
  created () {
    this.handleFilterClick()
  },
  mounted () {
    this.handleDropdownScroll()
  },
  methods: {
    /**
     * Check if the user has scrolled the dropdown to bottom. If true, emmit an event to parent indicating this.
     *
     * @return {void}
     */
    handleDropdownScroll () {
      this.$refs.dropdown.querySelector('.dropdown-content').addEventListener('scroll', debounce((e) => {
        if (this.hasReachedBottom(e.target)) {
          this.$emit('filter-bottom-was-reached')
        }
      }, 250))
    },

    /**
     * Check if the target was scrolled until bottom.
     *
     * @param  {object HTMLDocument}  target
     * @return {Boolean}
     */
    hasReachedBottom (target) {
      if (target.clientHeight + target.scrollTop === target.scrollHeight) {
        return true
      }

      return false
    },

    /**
     * Handle the click on the filter element. If the user clicks outside it, the modal is closed.
     *
     * @return {viod}
     */
    handleFilterClick () {
      window.addEventListener('click', (e) => {
        if (!this.isFilterElement(e.target)) {
          this.isOpen = false
        }
      })
    },

    /**
     * Check if the click happens on the filter dropdown or not.
     *
     * @param  {Event}  e
     * @return {Boolean}
     */
    isFilterElement (e) {
      if (!e.parentNode || e.parentNode.toString() === '[object HTMLDocument]') {
        return false
      }

      if (e.parentNode === this.$refs.dropdown) {
        return true
      }

      return this.isFilterElement(e.parentNode)
    },

    /**
     * Handle the click on the header items.
     *
     * @param  {Object} item
     * @param  {Event} e
     * @return {void}
     */
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

    /**
     * Handle the click on the body items.
     *
     * @param  {Object} item
     * @param  {Event} e
     * @return {void}
     */
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

    /**
     * Emmit an event to parent when the user is typing on the search field
     *
     * @param  {Event} e
     * @return {void}
     */
    userIsTyping (e) {
      this.$emit('filter-input', e.target.value)
    }
  },
  watch: {
    /**
     * Watch for change on the `isOpen` property, to send an event to parent when the filter is opened/closed.
     *
     * @param  {Boolean}  val
     * @param  {Boolean}  oldVal
     * @return {Boolean}
     */
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
