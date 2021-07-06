<template>
  <div class="home">
    {{ channel }}
    Counter: {{ counter }}
    Strict Counter: {{ strictCounter }}
    <div v-for="(message, index) of messages" :key="index">
      <div :class="{ valid: isValid(message), strictValid: isStrictValid(message) }">{{ message }}</div>
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
      client: undefined as undefined | Client,
      strictCounter: 0,
      counter: 0
    }
  },
  methods: {
    addMessage: function (message: string) {
      this.messages.push(message)
      if (this.messages.length > 10) this.messages.shift()

      if (this.isValid(message)) {
        const messageValue = Number(message)
        this.counter += messageValue
      }

      if (this.isStrictValid(message)) {
        const messageValue = Number(message)
        this.strictCounter += messageValue
      }
    },
    isValid: function (message: string) {
      return /^(\+|-)[0-9]*$/.test(message)
    },
    isStrictValid: function (message: string) {
      return /^(\+|-)2$/.test(message)
    }
  }
})
</script>

<style scoped>
  .valid {
    color: green;
  }

  .strictValid {
    color: gold;
  }
</style>
