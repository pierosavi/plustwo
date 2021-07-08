<template>
  <n-layout>
      <n-list>
        <n-list-item v-for="(message, index) of messages" :key="index">
          <!-- <n-thing title="Thing" title-extra="extra" description="description"> -->
          <!-- <n-thing title="Thing" title-extra="extra" description="description">
            test
          </n-thing> -->
          <n-text :type="message.isValid ? 'success' : 'default'" :depth="message.isValid ? 1 : 3">{{ message.value }}</n-text>
        </n-list-item>
      </n-list>
  </n-layout>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue'
import { NList, NListItem, NText, NLayout } from 'naive-ui'
import { Message } from '@/types'

export default defineComponent({
  name: 'Chatbox',
  components: {
    NList,
    NListItem,
    NText,
    NLayout
  },
  props: {
    messages: {
      type: Array as PropType<Message[]>,
      required: true
    }
  }
})
</script>
