<template>
  <div class="container">
    <canvas class="memeCanvas mx-auto hidden" height="258" width="498"></canvas>
    <div v-if="canvas_src && canvas_src.length > 0">
      <img :src="canvas_src" class="mx-auto" alt="">
    </div>
    <div v-else class="bg-gray-300 w-full rounded-lg h-64 flex">
      <span class="flex items-center justify-center mx-auto font-semibold text-gray-600">
        Meme not generated...
      </span>
    </div>
  </div>
</template>

<style>
@font-face {
  font-family: 'Century';
  src: url('~assets/CenturyExpanded.otf');
}
</style>

<script lang="ts">
import Vue from 'vue'

import { EventBus } from '~/plugins/events'

function loadImage (url: string) {
  return new Promise(r => { let i = new Image(); i.onload = (() => r(i)); i.src = url; });
}

export default Vue.extend({
  props: {
    src: String,
    title: String
  },
  data: () => ({
    d_title: '',
    d_src: '',
    canvas_src: ''
  }),
  methods: {
    download: function () {
      const link = document.createElement('a')
      link.download = this.title + '.png'
      link.href = document.getElementsByTagName('canvas')[0].toDataURL()
      link.click()
    },
    draw: async function () {
      let title = this.title
      if (this.title.length > 9) title = title.slice(0, 10)

      const canvas = this.$el.getElementsByTagName('canvas')[0]
      const ctx = canvas.getContext('2d')
      
      if (ctx === null) return

      ctx.clearRect(0, 0, canvas.width, canvas.height)
      this.canvas_src = ''
      
      const background = new Image()
      background.src = require('~/assets/image.png')

      background.onload = () => {

        ctx.globalCompositeOperation = 'source-over'
        ctx.drawImage(background, 0, 0)
        console.log('background')

        if (this.src.length > 0) {
          const size = 128
          const picture = new Image(size, size)
          picture.onload = () => {
            ctx.drawImage(picture, 350, 20, size, size)
            ctx.globalCompositeOperation = 'destination-over'
            ctx.fillStyle = 'rgba(200, 0, 0, 0.5)'
            ctx.fillRect(0, 0, 0, 0)
            
            ctx.globalCompositeOperation = 'source-over'
            ctx.font = (56 - this.title.length * 1.75) + 'px Century'
            ctx.fillStyle = 'white'
            ctx.textAlign = 'center'
            ctx.fillText(this.title.toUpperCase().split('').join(String.fromCharCode(8202)), 187.5, 155)
            this.canvas_src = document.getElementsByTagName('canvas')[0].toDataURL()
          }
          picture.src = this.src
        } else {
          
          ctx.globalCompositeOperation = 'source-over'
          ctx.font = (56 - this.title.length * 1.75) + 'px Century'
          ctx.fillStyle = 'white'
          ctx.textAlign = 'center'
            ctx.fillText(this.title.toUpperCase().split('').join(String.fromCharCode(8202)), 187.5, 155)

          this.canvas_src = document.getElementsByTagName('canvas')[0].toDataURL()
        }
      }
    }
  },
  watch: {
    title: {
      immediate: true,
      deep: true,
      handler: function (newValue) {
        this.canvas_src = ''
        if (newValue && newValue.length === 0) return
        if (!newValue) return
        this.draw()
      }
    },
    src: {
      immediate: true,
      deep: true,
      handler: function (newValue) {
        this.canvas_src = ''
        if (newValue && newValue.length === 0) return
        if (!newValue) return
        this.draw()
      }
    },
  },
  mounted: function () {
    EventBus.$on('process', this.draw)
    EventBus.$on('download', this.download)
  }
})
</script>