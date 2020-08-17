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
    <div class="container mx-auto w-1/2 mt-4">
      <Input placeholder="Enter text..." v-model="text" />
    </div>
    <Preview :src="src" v-on:click=openDialog />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

import Button from '~/components/Button.vue'
import Input from '~/components/Input.vue'

import Preview from '~/layouts/Preview.vue'

export default Vue.extend({
  components: {
    Button,
    Input,
    Preview
  },
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
    },
    getRandomItem: function () : string {
      return this.randoms_items[Math.floor(Math.random() * this.randoms_items.length)]
    }
  },
  data: () => ({
    src: '',
    text: '',
    randoms_items: [
      'Ness',
      'America',
      'this project',
      'Internet'
    ]
  }),
  mounted: function () {
    this.text = this.getRandomItem()
  }
})
</script>