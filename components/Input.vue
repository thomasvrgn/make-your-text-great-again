<template>
  <input type="text" name="" id="" class="bg-gray-200 py-2 px-4 w-full rounded text-sm focus:outline-none text-gray-700 focus:bg-gray-300 transition duration-200" v-on:input="updateValue($event.target.value)">
</template>

<script lang="ts">
import Vue from 'vue'
import { EventBus } from '~/plugins/events'

export default Vue.extend({
  methods: {
    updateValue: function (value: string) {
      this.$emit('input', value)
    }
  },
  mounted: function () {
    EventBus.$on('reset', (value: string) => {
      this.updateValue(value)
    })
  }
})
</script>
