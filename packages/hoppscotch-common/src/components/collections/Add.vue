<template>
  <SmartModal
    v-if="show"
    dialog
    :title="t('collection.new')"
    @close="hideModal"
  >
    <template #body>
      <div class="flex flex-col">
        <input
          id="selectLabelAdd"
          v-model="name"
          v-focus
          class="input floating-input"
          placeholder=" "
          type="text"
          autocomplete="off"
          @keyup.enter="addNewCollection"
        />
        <label for="selectLabelAdd">
          {{ t("action.label") }}
        </label>
      </div>
    </template>
    <template #footer>
      <span class="flex space-x-2">
        <ButtonPrimary
          :label="t('action.save')"
          :loading="loadingState"
          outline
          @click="addNewCollection"
        />
        <ButtonSecondary
          :label="t('action.cancel')"
          outline
          filled
          @click="hideModal"
        />
      </span>
    </template>
  </SmartModal>
</template>

<script setup lang="ts">
import { watch, ref } from "vue"
import { useToast } from "@composables/toast"
import { useI18n } from "@composables/i18n"

const toast = useToast()
const t = useI18n()

const props = withDefaults(
  defineProps<{
    show: boolean
    loadingState: boolean
  }>(),
  {
    show: false,
    loadingState: false,
  }
)

const emit = defineEmits<{
  (e: "submit", name: string): void
  (e: "hide-modal"): void
}>()

const name = ref("")

watch(
  () => props.show,
  (show) => {
    if (!show) {
      name.value = ""
    }
  }
)

const addNewCollection = () => {
  if (!name.value) {
    toast.error(t("collection.invalid_name"))
    return
  }

  emit("submit", name.value)
}

const hideModal = () => {
  name.value = ""
  emit("hide-modal")
}
</script>
