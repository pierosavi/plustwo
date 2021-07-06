<template>
  <div class="home">
    {{ channel }}
    <div v-for="(message, index) of messages" :key="index">
      {{ message }}
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import tmi from 'tmi.js'
export default defineComponent({
  name: 'Channel',
  created: function () {
    const client = new tmi.Client({
      channels: [channel]
    })

    // eslint-disable-next-line @typescript-eslint/no-explicit-any
    client.connect().catch((err: any) => {
      console.error(err)
    })

    client.on('message', (channel, tags, message, self) => {
      this.messages.push(message)
      console.log(message)
    })
  },
  computed: {
    channel: function () {
      return this.$route.params.channel
    }
  },
  data: function () {
    return {
      messages: [] as string[]
    }
  }
})
</script>
