<template>
    <n-layout>
      <n-layout-content content-style="padding: 24px">
        <router-view/>
      </n-layout-content>
    </n-layout>
</template>

<script>
import { defineComponent } from 'vue'
import { NLayout, NLayoutContent } from 'naive-ui'

export default defineComponent({
  name: 'App',
  components: {
    NLayout,
    NLayoutContent
  }
})
</script>

<style lang="scss">
</style>
