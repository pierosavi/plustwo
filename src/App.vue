<template>
    <n-config-provider v-model:theme="theme">
      <n-global-style />
      <n-layout>
        <n-layout-header bordered style="height: 64px; padding: 24px;">
            PLUSTWO
            <n-switch v-model:value="darkMode" />
        </n-layout-header>
        <n-layout-content content-style="padding: 24px">
          <router-view/>
        </n-layout-content>
      </n-layout>
    </n-config-provider>
</template>

<script>
import { defineComponent } from 'vue'
import { NLayout, NLayoutContent, NConfigProvider, NGlobalStyle, darkTheme, NLayoutHeader, NSwitch } from 'naive-ui'

export default defineComponent({
  name: 'App',
  components: {
    NLayout,
    NLayoutContent,
    NConfigProvider,
    NGlobalStyle,
    NLayoutHeader,
    NSwitch
  },
  data: function () {
    return {
      darkMode: true
    }
  },
  computed: {
    theme: function () {
      return this.darkMode ? darkTheme : null
    }
  }
})
</script>

<style lang="scss">
</style>
