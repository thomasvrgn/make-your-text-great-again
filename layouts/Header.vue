<template>
  <div class="container">
    <Button color="teal" v-on:click=openDialog>
      Upload photo
    </Button>
    <Button color="indigo">
      Process
    </Button>
    <img :src="src" alt="">
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import Button from '~/components/Button.vue'

export default Vue.extend({
  components: {
    Button
  },
  data: () => ({
    src: ''
  }),
  methods: {
    openDialog: function () {
      console.log('test')
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
    }
  }
})
</script>