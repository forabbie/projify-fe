<template>
  <div
    :id="id"
    tabindex="-1"
    aria-hidden="true"
    class="fixed top-0 left-0 right-0 z-50 hidden w-full p-4 overflow-x-hidden overflow-y-auto md:inset-0 h-[calc(100%-1rem)] max-h-full"
    data-modal-backdrop="static"
  >
    <div class="relative w-full max-w-md max-h-full">
      <div class="relative bg-white rounded-lg shadow dark:bg-gray-700">
        <button
          type="button"
          class="absolute top-3 right-2.5 text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ml-auto inline-flex justify-center items-center dark:hover:bg-gray-600 dark:hover:text-white"
          :data-modal-hide="id"
        >
          <svg
            class="w-3 h-3"
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 14 14"
          >
            <path
              stroke="currentColor"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6"
            />
          </svg>
          <span class="sr-only">Close modal</span>
        </button>
        <div class="px-6 py-6 lg:px-8">
          <h3 class="mb-4 text-xl font-medium text-gray-900 dark:text-white">Invite user</h3>
          <vee-form class="mt-8 space-y-6" :validation-schema="schema" @submit="onsave">
            <div
              class="text-white text-center font-bold p-4 mb-4"
              v-if="show_alert"
              :class="alert_variant"
            >
              {{ alert_msg }}
            </div>
            <FieldInputGroup>
              <template #label>
                <FieldInputLabel label="Email" label_for="input-recipient" />
              </template>
              <template #field>
                <FieldInput
                  :data="data.recipient_email"
                  type="text"
                  name="recipient"
                  id="input-recipient"
                  placeholder="workspace@example.com"
                  :class="inputTextClass"
                />
              </template>
              <template #error>
                <ErrorMessage class="text-red-600" name="recipient" />
              </template>
            </FieldInputGroup>
            <button
              type="submit"
              class="w-full text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
              style="transition: all 0.15s ease 0s"
              :disabled="submission"
            >
              Invite
            </button>
          </vee-form>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue'
import FieldInput from '@/components/elements/FieldInput.vue'
import FieldInputGroup from '@/components/elements/FieldInputGroup.vue'
import FieldInputLabel from '@/components/elements/FieldInputLabel.vue'
import useMemberStore from '@/stores/member'

const emit = defineEmits(['onAction'])

const memberStore = useMemberStore()
const props = defineProps({
  id: String,
  data: Object,
  label: String,
  onAction: Function
})
const id = ref(props.id)
// const member = ref(props.data)

const inputTextClass = ref(
  'bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500'
)

const schema = ref(
  computed(() => {
    return memberStore.schema
  })
)
const submission = ref(computed(() => memberStore.submission))
const show_alert = ref(computed(() => memberStore.show_alert))
const alert_variant = ref(computed(() => memberStore.alert_variant))
const alert_msg = ref(computed(() => memberStore.alert_msg))

const onsave = (value) => {
  emit('onAction', value)
}
</script>
