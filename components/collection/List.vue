<template>
  <div
    class="row-span-3 bg-gray-50 border-r p-5 rounded-l-lg h-[calc(100vh-150px)] overflow-auto"
  >
    <div class="pb-3 w-[100%]">
      <!-- Adding template to the list -->
      <button
        class="bg-white hover:bg-gray-50 hover:text-gray-800 border focus-visible:outline focus-visible:outline-2 focus-visible:outline-indigo-600 focus-visible:outline-offset-2 font-semibold inline-flex items-center justify-center px-3 py-3 rounded-md shadow-sm text-gray-600 text-sm w-[100%]"
        @click="emit('showTemplateForm', true)"
      >
        <span>
          <!-- <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="w-6 h-6"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M12 4.5v15m7.5-7.5h-15"
            />
          </svg> -->
        </span>
        Add Template
      </button>
    </div>
    <div v-for="(email, index) in emailTemplates">
      <div
        class="border-b border-gray-200 bg-white px-4 py-5 sm:px-6 group/item justify-between flex"
      >
        <div>
          <h3 class="text-base font-semibold leading-6 text-gray-900">
            {{ email.name }}
          </h3>
          <p class="mt-1 text-sm text-gray-500">{{ email.subject }}</p>
        </div>
        <div class="group-hover/item:visible invisible flex items-center">
          <PencilIcon
            class="h-5 w-5 cursor-pointer mr-3"
            @click="$emit('editTemplate', email, index)"
          />
          <TrashIcon
            class="h-5 w-5 cursor-pointer tect-danger"
            @click="deleteTemplate(email, index)"
          />
        </div>
      </div>
    </div>
    <TransitionRoot as="template" :show="open">
      <Dialog as="div" class="relative z-10" @close="open = false">
        <TransitionChild
          as="template"
          enter="ease-out duration-300"
          enter-from="opacity-0"
          enter-to="opacity-100"
          leave="ease-in duration-200"
          leave-from="opacity-100"
          leave-to="opacity-0"
        >
          <div
            class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"
          />
        </TransitionChild>

        <div class="fixed inset-0 z-10 overflow-y-auto">
          <div
            class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0"
          >
            <TransitionChild
              as="template"
              enter="ease-out duration-300"
              enter-from="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
              enter-to="opacity-100 translate-y-0 sm:scale-100"
              leave="ease-in duration-200"
              leave-from="opacity-100 translate-y-0 sm:scale-100"
              leave-to="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
            >
              <DialogPanel
                class="relative transform overflow-hidden rounded-lg bg-white px-4 pb-4 pt-5 text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg sm:p-6"
              >
                <div>
                  <div
                    class="mx-auto flex h-12 w-12 items-center justify-center rounded-full bg-red-100"
                  >
                    <TrashIcon
                      class="h-6 w-6 text-red-600"
                      aria-hidden="true"
                    />
                  </div>
                  <div class="mt-3 text-center sm:mt-5">
                    <DialogTitle
                      as="h3"
                      class="text-base font-semibold leading-6 text-gray-900"
                      >Are you sure want to delete ?</DialogTitle
                    >
                  </div>
                </div>
                <div
                  class="mt-5 sm:mt-6 sm:grid sm:grid-flow-row-dense sm:grid-cols-2 sm:gap-3"
                >
                  <button
                    type="button"
                    class="inline-flex w-full justify-center rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 sm:col-start-2"
                    @click="deleteModal"
                  >
                    Delete
                  </button>
                  <button
                    type="button"
                    class="mt-3 inline-flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:col-start-1 sm:mt-0"
                    @click="open = false"
                    ref="cancelButtonRef"
                  >
                    cancel
                  </button>
                </div>
              </DialogPanel>
            </TransitionChild>
          </div>
        </div>
      </Dialog>
    </TransitionRoot>
  </div>
</template>
<script setup lang="ts">
import { defineProps, defineEmits } from "vue";
import { PencilIcon, TrashIcon } from "@heroicons/vue/24/outline";
import { ref } from "vue";
import {
  Dialog,
  DialogPanel,
  DialogTitle,
  TransitionChild,
  TransitionRoot,
} from "@headlessui/vue";
import { CheckIcon } from "@heroicons/vue/24/outline";
const open = ref(false);
const selectedEmail = ref({});
const selectedIndex = ref(0);

const emit = defineEmits([
  "DeleteSavedTemplate",
  "showTemplateForm",
  "editTemplate",
]);

const props = defineProps({
  // Declare props
  emailTemplates: Array,
});
const deleteTemplate = (email: Object, index: Number) => {
  open.value = true;
  selectedEmail.value = email;
  selectedIndex.value = index;
};
const deleteModal = () => {
  open.value = false;
  emit("DeleteSavedTemplate", {
    email: selectedEmail.value,
    index: selectedIndex.value,
  });
};
</script>
