<template>
  <div 
  id="app"
    class="dark:(bg-dark-bg text-dark-text) bg-light-bg min-h-screen text-light-text max-w-screen flex flex-col overflow-hidden"
    :style="getBgPattern()">
    <Header />
    <router-view class="container p-2 my-8 mx-auto" />
    <Footer class="mt-auto" />
  </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
import { useI18n } from 'vue-i18n'

import { useHead } from '@vueuse/head'
import { isDark } from '~/utils'
export default defineComponent({
  setup() {
    const { t } = useI18n()
    useHead({
      title: 'wappy 💌',
      meta: [{ name: 'description', content: t('meta.description') }]
    })

    // Todo: Find a better way
    const getBgPattern = function () {
      return isDark.value
        ? 'background-image: linear-gradient(rgba(19, 28, 33, 0.9), rgba(19, 28, 33, 0.9)), url(bg-dark.png);'
        : 'background-image: linear-gradient(rgba(223, 216, 208, 0.9), rgba(223, 216, 208, 0.9)), url(bg-light.png);'
    }
    return { isDark, getBgPattern }
  }
})
</script>
<style>
::selection {
  color: #000;
  background: #25d366;
}
</style>