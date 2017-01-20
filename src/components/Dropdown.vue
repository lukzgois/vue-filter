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
  mounted () {
    document.querySelector('.dropdown-content').addEventListener('scroll', (e) => {
      let target = e.target
      if (target.clientHeight + target.scrollTop === target.scrollHeight) {
        this.$emit('filter-bottom-was-reached')
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
