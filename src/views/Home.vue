<template>
  <div class="home">
    <n-space>
      <n-input v-model:value="channel" type="input" placeholder="Channel name" @keypress.enter="$router.push({ name: 'Channel', params: { channel }})" />
      <n-button type="primary" :disabled="!channel" @click="$router.push({ name: 'Channel', params: { channel }})">Go to channel</n-button>
    </n-space>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { NInput, NSpace, NButton } from 'naive-ui'

export default defineComponent({
  name: 'Home',
  components: {
    NInput,
    NSpace,
    NButton
  },
  data: function () {
    return {
      baseUrl: window.location.origin,
      channel: ''
    }
  }
})
</script>
