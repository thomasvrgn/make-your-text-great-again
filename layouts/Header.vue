<template>
  <div class="container mx-auto my-16">
    <h1 class="text-center mb-16 text-5xl font-bold text-gray-700">
      Make {{ text }} great again!
    </h1>
    <div class="container mx-auto w-1/2 flex justify-center space-x-4">
      <Button class="flex-auto" color="indigo" v-on:click=openDialog>
        Upload photo
      </Button>
      <Button class="flex-auto" color="red" v-on:click=resetDialog>
        Reset
      </Button>
    </div>
    <div class="container mx-auto w-1/2 mt-4">
      <Input placeholder="Enter text..." v-model="text" />
    </div>
    <div class="lg:container lg:mx-auto lg:flex lg:items-center lg:mx-auto lg:justify-center">
      <Preview :src="src" v-on:click=openDialog class="lg:flex-auto lg:w-5/12" />
      <Canvas :src="src" :title="text" class="lg:flex-auto lg:w-1/2" ref="process" />
    </div>
    <Button color="indigo" class="mx-auto block" v-on:click=download>
      Download
    </Button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

import Button from '~/components/Button.vue'
import Input from '~/components/Input.vue'

import Preview from '~/layouts/Preview.vue'
import Canvas from '~/layouts/Canvas.vue'

import { EventBus } from '~/plugins/events'

export default Vue.extend({
  components: {
    Button,
    Input,

    Preview,
    Canvas
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
        this.disabled = !this.disabled

        setTimeout(() => {
          this.disabled = !this.disabled
        }, 1000)
        input.remove()
      })
    },
    resetDialog: function () {
      this.src = ''
    },
    getRandomItem: function () : string {
      return this.randoms_items[Math.floor(Math.random() * this.randoms_items.length)]
    },
    download: function () {
      EventBus.$emit('download')
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
    ],
    disabled: false
  }),
  mounted: function () {
    this.text = this.getRandomItem()
    document.title = `Make ${this.text} great again!`
  },
  watch: {
    text: {
      immediate: true,
      deep: true,
      handler: function () {
        document.title = `Make ${this.text} great again!`
      }
    }
  }
})
</script>