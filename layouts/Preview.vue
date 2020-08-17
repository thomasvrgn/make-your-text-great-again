<template>
  <div class="w-1/2 block bg-gray-300 mx-auto rounded-lg my-16">
    <div class="container h-64 flex" v-if="src.length === 0">
      <span class="flex flex-initial text-center mx-auto items-center justify-center font-semibold text-gray-600">
        No images were uploaded...
      </span>
    </div>
    <div v-else class="py-16 mx-16">
      <img :src="src" class="h-64 mx-auto" alt="">
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  props: {
    src: String
  }
})
</script>