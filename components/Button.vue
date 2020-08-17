<template>
  <button class="py-2 px-6 font-semibold rounded text-sm focus:outline-none transition duration-200" :class="`${disabledClass}`" @click="$emit('click')">
    <slot></slot>
  </button>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  props: {
    color: String,
    disabled: Boolean
  },
  data: () => ({
    disabledClass: ''
  }),
  watch: {
    disabled: {
      immediate: true,
      deep: true,
      handler: function () {
        if (!this.disabled) return this.disabledClass = `bg-${this.color}-500 hover:bg-${this.color}-600 text-white`
        this.disabledClass = 'bg-gray-300 pointer-events-none text-gray-700'
        return
      }
    }
  }
})
</script>