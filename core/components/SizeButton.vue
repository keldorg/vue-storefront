<template>
  <button>
    <!--
        Filter button for category size
        props:
        "label" - text displayed at filter
        "id" - filter ID
        "code" - filter code
        "context"
        data to display:
        {{ label }}
      -->
  </button>
</template>

<script>
export default {
  name: 'SizeButton',
  props: {
    label: {
      type: null,
      required: false,
      default: () => false
    },
    id: {
      type: null,
      required: false,
      default: () => false
    },
    code: {
      type: null,
      required: false,
      default: () => false
    },
    context: {
      type: null,
      required: false,
      default: () => false
    }
  },
  data () {
    return {
      active: false
    }
  },
  beforeDestroy () {
    this.$bus.$off('filter-reset')
    this.$bus.$off('filter-changed-' + this.context)
  },
  beforeMount () {
    this.$bus.$on('filter-reset', this.filterReset)
    this.$bus.$on('filter-changed-' + this.context, this.filterChanged)
  },
  methods: {
    filterChanged (filterOption) {
      if (filterOption.attribute_code === this.code) {
        if (filterOption.id === this.id) {
          if (this.active) {
            this.active = false
          } else {
            this.active = true
          }
        } else {
          this.active = false
        }
        // filterOption.id === this.id ? this.active = true : this.active = false
      }
    },
    filterReset (filterOption) {
      this.active = false
    },
    switchFilter (id, label) {
      this.$bus.$emit('filter-changed-' + this.context, { attribute_code: this.code, id: id, label: label })
    }
  }
}
</script>
