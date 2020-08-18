<template>
  <button class="py-2 px-6 font-semibold rounded text-sm focus:outline-none transition duration-200 text-white" :class="{'bg-indigo-500 hover:bg-indigo-600': color === 'indigo', 'bg-red-500 hover:bg-red-600': color === 'red'}" @click="$emit('click')">
    <slot></slot>
  </button>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  props: {
    color: String,
    disabled: Boolean
  }
})
</script>