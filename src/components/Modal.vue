<template>
  <teleport to="#modals">
    <transition leave-active-class="duration-200">
      <div
        v-show="show"
        class="fixed inset-0 z-10 overflow-y-auto"
      >
        <div class="flex items-end justify-center min-h-screen px-4 pt-4 pb-20 text-center sm:block sm:p-0">
          <transition
            enter-active-class="duration-300 ease-out"
            enter-from-class="opacity-0"
            enter-to-class="opacity-100"
            leave-active-class="duration-200 ease-in"
            leave-from-class="opacity-100"
            leave-to-class="opacity-0"
          >
            <div
              v-show="show"
              class="fixed inset-0 transition-opacity"
              aria-hidden="true"
              @click="close"
            >
              <div class="absolute inset-0 bg-gray-100 opacity-90 dark:bg-black" />
            </div>
          </transition>

          <!-- This element is to trick the browser into centering the modal contents. -->
          <span class="hidden sm:inline-block sm:align-middle sm:h-screen" aria-hidden="true">&#8203;</span>

          <transition
            enter-active-class="duration-300 ease-out"
            enter-from-class="translate-y-4 opacity-0 sm:translate-y-0 sm:scale-95"
            enter-to-class="translate-y-0 opacity-100 sm:scale-100"
            leave-active-class="duration-200 ease-in"
            leave-from-class="translate-y-0 opacity-100 sm:scale-100"
            leave-to-class="translate-y-4 opacity-0 sm:translate-y-0 sm:scale-95"
          >
            <div
              v-show="show"
              class="inline-block w-full overflow-hidden text-left align-bottom transition-all transform bg-white rounded-lg shadow-xl dark:bg-gray-900 sm:my-8 sm:align-middle sm:max-w-lg"
              role="dialog"
              aria-modal="true"
              aria-labelledby="modal-headline"
            >
              <div class="p-4 bg-white dark:bg-gray-900 sm:p-6">
                <div class="flex items-start space-x-4 sm:space-x-6">
                  <div class="hidden sm:block">
                    <slot name="icon" />
                  </div>

                  <div class="flex-1">
                    <header class="flex items-center">
                      <h3 class="flex items-center h-12 text-lg font-semibold leading-6 text-gray-600 dark:text-gray-200 sm:h-10" id="modal-headline">
                        <slot name="title" />
                      </h3>
                    </header>

                    <div class="mt-3">
                      <slot name="content" />
                    </div>
                  </div>
                </div>
              </div>
              <div class="px-4 py-3 bg-gray-100 dark:bg-gray-800 sm:px-6 sm:flex sm:flex-row-reverse">
                <slot name="actions" />
              </div>
            </div>
          </transition>
        </div>
      </div>
    </transition>
  </teleport>
</template>

<script>
import { inject, onMounted, onUnmounted } from 'vue'
import hotkeys from 'hotkeys-js'

export default {
  setup() {
    const show = inject('show')
    const close = () => show.value = false

    onMounted(() => {
      hotkeys('esc', (event) => {
        show.value = false
        event.preventDefault()
      })
    })

    onUnmounted(() => hotkeys.unbind('esc'))

    return {
      close,
      show,
    }
  }
}
</script>
