<template>
  <div class="my-auto space-y-4 w-full h-full p-4 rounded-md shadow-lg flex flex-col
    dark:bg-dark-bglayer-2 bg-light-bglayer-2">
    <label class="font-bold px-1 text-2xl" :style="`color: ${stringToHSL(userstats.username)};`">
      {{ userstats.username }} {{ getProfileEmoji() }}
    </label>

    <div class="p-4 dark:bg-dark-bgmessage_sender_quote bg-light-bgmessage_sender_quote rounded-lg shadow-md">
      <div class="flex items-center">
        <div class="flex flex-col">
          <span class="text-2xl mb-0"> {{ t('stats.messages') }}</span>
          <span class="text-base text-light-label text-sm dark:text-dark-label pb-1">
            <span class="text-accent"> {{ getPercentage(globalstats.count, userstats.messagesCount) }}%</span>
              {{ t('stats.user.percentageMessages') }}
            </span>
        </div>
        <div class="text-4xl ml-auto text-center items-end flex font-bold text-primarylight">
          {{ userstats.messagesCount }}
        </div>
      </div>
      <hr class="border-t-1 my-2 dark:border-primarydarker border-primarylight" />

      <div class="flex items-center">
        <div class="flex flex-col">
          <span class="text-2xl mb-0">{{ t('stats.media') }}</span>
          <span class="text-base text-light-label text-sm dark:text-dark-label pb-1">
            <span class="text-accent"> {{ getPercentage(userstats.messagesCount, userstats.mediaCount) }}%</span>
            {{ t('stats.user.percentageMedia') }}
          </span>
        </div>
        <div class="text-4xl ml-auto text-center items-end flex font-bold text-primarylight">
          {{ userstats.mediaCount }}
        </div>
      </div>
    </div>
    <div v-if="userstats.responseTime > 0"
      class="p-2 rounded-lg w-full self-end border-2 shadow-md 
        dark:bg-dark-bgmessage_sender_quote dark:border-dark-bgmessage_sender 
        bg-light-bgmessage_sender_quote border-light-bgmessage_sender">
      <div class="rounded-lg flex">
        <i-bx-bx-timer class="text-2xl mx-2 my-auto text-primarylight" />
        <span class="text-lg">
          {{ t('stats.user.average') }} <span class="text-accent">{{ formatTime(userstats.responseTime) }}</span>
        </span>
      </div>
    </div>
    <div class="p-2 rounded-lg w-full self-end border-2 shadow-md dark:bg-dark-bgmessage_sender_quote 
      dark:border-dark-bgmessage_sender bg-light-bgmessage_sender_quote border-light-bgmessage_sender">
      <div class="rounded-lg flex">
        <i-dashicons-format-chat class="text-2xl mx-2 my-auto text-primarylight" />
        <span class="text-lg" v-html="t('stats.user.startedConversations', [userstats.startedConversations])"> </span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { stringToHSL, formatTime, getPercentage, getProfileEmoji } from '~/utils'
import { useI18n } from 'vue-i18n'
export default defineComponent({
  props: {
    globalstats: {
      type: Object,
      required: true
    },
    userstats: {
      type: Object,
      required: true
    }
  },
  setup() {
    const { t } = useI18n()
    return { stringToHSL, t, formatTime, getPercentage, getProfileEmoji }
  }
})
</script>