<template>
  <div class="container mx-auto">
    <Button color="teal" v-on:click=openDialog>
      Upload photo
    </Button>
    <Button color="indigo">
      Process
    </Button>
    <Button color="red" v-on:click=resetDialog>
      Reset
    </Button>
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
    src: ''
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