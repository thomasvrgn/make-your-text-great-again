<template>
  <div class="container mx-auto my-16">
    <h1 class="text-center mb-16 text-5xl font-bold text-gray-700">
      Make {{ text }} great again!
    </h1>
    <div class="container mx-auto w-1/2 flex justify-center space-x-4">
      <Button class="flex-auto" color="indigo" v-on:click=openDialog>
        Upload photo
      </Button>
      <Button class="flex-auto" color="teal">
        Process
      </Button>
      <Button class="flex-auto" color="red" v-on:click=resetDialog>
        Reset
      </Button>
    </div>
    <Preview :src="src" />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import Button from '~/components/Button.vue'
import Preview from '~/layouts/Preview.vue'

export default Vue.extend({
  components: {
    Button,
    Preview
  },
  data: () => ({
    src: '',
    text: 'Your Text'
  }),
  methods: {
    openDialog: function () {
      const input = document.createElement('input')
      input.setAttribute('type', 'file')
      input.click()
      input.addEventListener('change', () => {
        if (input.files === null) return
        const file = input.files[0]
        if (!file.type.startsWith('image')) return
        this.src = URL.createObjectURL(file)
        input.remove()
      })
    },
    resetDialog: function () {
      this.src = ''
    }
  }
})
</script>