<template>
  <button class="py-2 px-6 text-white font-semibold rounded text-sm focus:outline-none" :class="`bg-${color}-500 hover:bg-${color}-600 focus:bg-${color}-700`">
    <slot></slot>
  </button>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  props: {
    color: String
  }  
})
</script>