<template>
  <div
    @dragover.prevent
    @drop.prevent
    @drop="handleFileDrop"
    class="rounded-xl shadow-lg font-semibold text-md dark:bg-dark-bglayer-1 dark:hover:bg-dark-bglayer-2 dark:text-dark-label dark:border-primarydark bg-light-bglayer-2 hover:bg-light-bglayer-3 border-primarylight text-light-label"
  >
    <div class="rounded-xl p-8 cursor-pointer text-center items-center" @click="inputClick">
      <div v-if="!loading">
        <div class="transform scale-x-n1">
          <i-ant-design-paper-clip-outlined class="text-6xl block mx-auto dark:text-dark-icon text-light-icon" />
        </div>
        <span class="dark:text-dark-text text-light-text"> {{ t('uploader.title') }} </span>
        <input ref="fileInput" class="hidden" type="file" @change="handleFileInput" accept=".txt, .zip" />
      </div>

      <div v-else>
        <div v-if="loading" class="animate-spin m-2 text-4xl h-11 w-11 block mx-auto">
          <i-mdi-loading class="dark:text-dark-icon text-light-icon" />
        </div>
        <span class="my-auto text-2xl italic font-light cursor-text dark:text-dark-text text-light-text">
          {{ t('uploader.loading') }}
        </span>
      </div>
    </div>
    <div
      v-if="fileSelected"
      class="px-8 py-4 rounded-b-xl text-sm border-t-2 border-dashed dark:border-primarydark border-primarylight"
    >
      <span class="my-auto">{{ fileSelected.name }} | {{ Math.round(fileSelected.size / 1024) || '>1' }} KB</span>
    </div>
  </div>
</template>

<script lang="ts">
import { ref, defineComponent } from 'vue'
import { useI18n } from 'vue-i18n'

export default defineComponent({
  props: {
    loading: Boolean,
    error: Error
  },
  setup({ loading }, { emit }) {
    let fileSelected = ref()
    const fileInput = ref()

    const inputClick = function () {
      fileInput.value.click()
    }

    const handleFileDrop = function (e) {
      if (!loading) {
        let droppedFiles = e.dataTransfer.files
        if (!droppedFiles) return
        ;[...droppedFiles].forEach((f) => {
          console.log(f)
          fileSelected.value = f
          emit('uploaded', fileSelected.value)
        })
      }
    }

    const handleFileInput = function (e) {
      if (!loading) {
        let files = e.target.files
        if (!files) return
        fileSelected.value = files[0]
        emit('uploaded', fileSelected.value)
      }
    }

    const { t } = useI18n()

    return { fileSelected, inputClick, fileInput, handleFileDrop, handleFileInput, t }
  }
})
</script>