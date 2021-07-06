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
import tmi, { Client } from 'tmi.js'
export default defineComponent({
  name: 'Channel',
  created: function () {
    this.client = new tmi.Client({
      channels: [this.channel]
    })

    // eslint-disable-next-line @typescript-eslint/no-explicit-any
    this.client.connect().then(() => {
      console.log('connected')
    }).catch((err: unknown) => {
      console.error(err)
    })

    this.client.on('message', (channel, tags, message, self) => {
      this.addMessage(message)
    })
  },
  computed: {
    channel: function (): string {
      const channelParam = this.$route.params.channel
      return Array.isArray(channelParam) ? channelParam[0] : channelParam
    }
  },
  data: function () {
    return {
      messages: [] as string[],
      client: undefined as undefined | Client
    }
  },
  methods: {
    addMessage: function (message: string) {
      this.messages.push(message)
      if (this.messages.length > 10) this.messages.shift()
    }
  }
})
</script>
