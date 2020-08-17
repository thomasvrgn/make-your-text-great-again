<template>
  <div class="container">
    <canvas class="memeCanvas mx-auto" height="349" width="640"></canvas>
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

export default Vue.extend({
  props: {
    src: String,
    title: String
  },
  methods: {
    draw: function () {
      let title = this.title
      if (this.title.length > 9) title = title.slice(0, 10)
      const canvas = this.$el.getElementsByTagName('canvas')[0]
      const ctx = canvas.getContext('2d')

      if (ctx === null) return

      ctx.clearRect(0, 0, canvas.width, canvas.height)
      const background = new Image()
      background.src = require('~/assets/image.png')

      background.addEventListener('load', function () {
        ctx.drawImage(background, 0, 0)
      })

      const size = 128

      const picture = new Image(size, size)
      picture.src = this.src

      picture.addEventListener('load', function () {
        ctx.drawImage(picture, 410, 60, size, size)
      })

      ctx.font = '48px Century'
      ctx.fillStyle = 'white'
      ctx.globalCompositeOperation='destination-over'
      ctx.textAlign = 'center'
      ctx.fillText(title.toUpperCase().split('').join(String.fromCharCode(8202)), 255, 195)
    }
  },
  watch: {
    title: {
      immediate: true,
      deep: true,
      handler: function (newValue) {
        if (newValue.length === 0) return
        this.draw()
      }
    },
    src: {
      immediate: true,
      deep: true,
      handler: function (newValue) {
        if (newValue.length === 0) return
        this.draw()
      }
    },
  },
  mounted: function () {
    EventBus.$on('process', this.draw)
  }
})
</script>