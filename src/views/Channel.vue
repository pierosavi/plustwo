<template>
  <div class="home">
    <n-grid x-gap="12" cols="3 s:1 m:3 l:3 xl:3 2xl:3" responsive="screen">
      <n-grid-item>
        <n-card title="Counter">
          {{ counter }}
          <Chart :data="counterHistory" />
        </n-card>

      </n-grid-item>
      <n-grid-item>
        <n-card title="Strict Counter">
          {{ strictCounter }}
          <Chart :data="strictCounterHistory" />
        </n-card>
      </n-grid-item>
      <n-grid-item>
        <n-card title="Chat">
          <template #header-extra>
            <n-text :type="chatStatus.type">{{ chatStatus.value }}</n-text>
          </template>
          <Chatbox :messages="messages" />
        </n-card>
      </n-grid-item>
    </n-grid>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import tmi, { Client } from 'tmi.js'
import Chatbox from '@/components/Chatbox.vue'
import Chart from '@/components/Chart.vue'
import { Message } from '@/types'
import { NGrid, NGridItem, NCard, NText } from 'naive-ui'

const CHAT_STATUS = {
  connecting: {
    type: 'warning',
    value: 'Connecting...'
  },
  connected: {
    type: 'success',
    value: 'Connected'
  },
  error: {
    type: 'error',
    value: 'Error'
  }
}

export default defineComponent({
  name: 'Channel',
  components: {
    Chatbox,
    NGrid,
    NGridItem,
    NCard,
    NText,
    Chart
  },
  created: function () {
    this.chatStatus = CHAT_STATUS.connecting

    this.client = new tmi.Client({
      channels: [this.channel]
    })

    this.client.connect().then(() => {
      this.chatStatus = CHAT_STATUS.connected
    }).catch((err: unknown) => {
      this.chatStatus = CHAT_STATUS.error
      console.error(err)
    })

    // eslint-disable-next-line @typescript-eslint/no-unused-vars
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
      messages: [] as Message[],
      client: undefined as undefined | Client,
      strictCounter: 0,
      counter: 0,
      strictCounterHistory: [{
        x: new Date(),
        y: 0
      }],
      counterHistory: [{
        x: new Date(),
        y: 0
      }],
      CHAT_STATUS,
      chatStatus: CHAT_STATUS.connecting
    }
  },
  methods: {
    addMessage: function (value: string) {
      if (this.messages.length > 10) this.messages.shift()

      const message = {
        value,
        isValid: false,
        isStrictValid: false
      } as Message

      if (this.isValid(message.value)) {
        const messageValue = Number(message.value)
        this.counter += messageValue
        message.isValid = true
        this.counterHistory.push({
          x: new Date(),
          y: this.counter
        })
      }

      if (this.isStrictValid(message.value)) {
        const messageValue = Number(message.value)
        this.strictCounter += messageValue
        message.isStrictValid = true
        this.strictCounterHistory.push({
          x: new Date(),
          y: this.strictCounter
        })
      }

      this.messages.push(message)
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
</style>
