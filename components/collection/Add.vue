<template>
  <div>
    <!-- Input field for Template Subject -->
    <div class="row-span-3 col-span-4 bg-white h-[calc(100vh-150px)]">
      <div class="bg-gray-50 mx-auto px-5 py-3">
        <div class="text-center mb-0 rounded-0">
          <!-- Input field for Template name -->
          <div class="flex justify-between items-center">
            <input
              id="email"
              v-model="emailForm.name"
              type="text"
              name="email"
              class="block rounded-md border-0 py-2 px-3 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6 w-[100%]"
              placeholder="Enter Template Name"
              required
            />
          </div>
        </div>
      </div>
      <div class="mx-4 mt-4">
        <input
          id="email"
          v-model="emailForm.subject"
          type="text"
          name="email"
          class="block mb-3 px-3 rounded-md border-0 py-2 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6 w-[100%]"
          placeholder="Enter Subject"
          required
        />
        <!-- Textarea for template body -->
        <textarea
          v-model="emailForm.body"
          rows="4"
          class="p-4 h-[calc(100vh-350px)] block w-full rounded-md border-0 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-300 sm:py-1.5 sm:text-sm sm:leading-6"
          placeholder="Add Template Body..."
          required
        />
        <!-- Buttons for template  -->
        <div class="flex justify-end mr-3 mt-4">
          <button
            type="button"
            class="border rounded-md bg-white py-2 px-3 text-sm font-semibold text-gray-600 shadow-sm hover:bg-gray-50 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 mr-3"
            @click="emit('hideTemplateForm')"
          >
            Cancel
          </button>
          <button
            type="button"
            class="rounded-md bg-indigo-600 py-2 px-4 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
            @click="saveTemplate()"
          >
            Save
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { defineEmits, defineProps } from "vue";
const emit = defineEmits(["savedTemplateData", "hideTemplateForm"]);

const props = defineProps({
  // Declare props
  emailForm: Object,
});
const saveTemplate = async () => {
  console.log("emailForm", props.emailForm);
  let data = {
    project_id: "111",
    name: props.emailForm.name,
    subject: props.emailForm.subject,
    body: props.emailForm.body,
    is_active: "1",
    type: "PLAIN_TEXT",
    share_type: "PRIVATE",
    category: "category1",
  };
  if (props.emailForm && props.emailForm.uid) data["uid"] = props.emailForm.uid;
  emit("savedTemplateData", data);
};
</script>
