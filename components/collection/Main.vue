<template>
  <div
    class="grid grid-rows-3 grid-flow-col grid-cols-4 border w-[80vw] mx-auto my-5 rounded-lg pr-[4px]"
  >
    <CollectionList
      :emailTemplates="emailTemplates"
      @DeleteSavedTemplate="DeleteSavedTemplate"
      @showTemplateForm="showTemplateForm"
      @editTemplate="editTemplate"
    />
    <div
      v-if="showTemplateFields"
      class="row-span-3 col-span-4 bg-white h-[calc(100vh-150px)]"
    >
      <CollectionAdd
        @savedTemplateData="savedTemplateData"
        @hideTemplateForm="showTemplateFields = false"
        :emailForm="Form"
      />
    </div>
    <!-- <div
      class="absolute top-[23rem] left-[49rem] font-extrabold text-3xl"
      v-else
    >
      <div class="pb-3 w-[100%]">
        <button
          class="bg-white hover:bg-gray-50 hover:text-gray-800 border focus-visible:outline focus-visible:outline-2 focus-visible:outline-indigo-600 focus-visible:outline-offset-2 font-semibold inline-flex items-center justify-center px-3 py-3 rounded-md shadow-sm text-gray-600 text-sm w-[100%]"
          @click="showTemplateForm()"
        >
          Add Template
        </button>
      </div>
      Please click on add button to add email template
    </div> -->
  </div>
</template>
<script setup lang="ts">
const showTemplateFields = ref(false);
const EamilIndex = ref(0);
const Form = ref({
  name: "",
  subject: "",
  body: "",
});
// Import vue
import { ref } from "vue";
import {
  useAuthLazyFetch,
  useAuthLazyFetchPost,
  useAuthLazyFetchDelete,
  useAuthLazyFetchPut,
} from "~/composables/useAuthLazyFetch";
const { pending, data: emailTemplates } = await useAuthLazyFetch(
  "https://v1-orm-lib.mars.hipso.cc/email-templates/?offset=0&limit=100&sort_column=id&sort_direction=desc"
);
const savedTemplateData = async (data: Object) => {
  if (data.uid) updateData(data);
  else {
    let options = {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        Accept: "application/json",
        Authorization:
          "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiYzljMGM3MjkzMWI0NGZjOWE1NTc5ZWMyOTg4NzVlYzMiLCJkIjoiMTY4MDA2MiIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMyNjczNzl9.3_zIDyeZ0ACxOF0VpywmxGpzdhUadzmvMRggb106s5E",
      },
      body: JSON.stringify(data),
    };
    const addTemplateData = await useAuthLazyFetchPost(
      "https://v1-orm-lib.mars.hipso.cc/email-templates/",
      options
    );
    emailTemplates.value.unshift(addTemplateData.data.value);
    Form.value = {};
  }
  showTemplateFields.value = false;
};
const DeleteSavedTemplate = async (data: Object) => {
  const response = await useAuthLazyFetchDelete(
    `https://v1-orm-lib.mars.hipso.cc/email-templates/${data.email.uid}`,
    {}
  );
  if (response.data.value) emailTemplates.value.splice(data.index, 1);
};
const showTemplateForm = () => {
  Form.value = {};
  showTemplateFields.value = true;
};
const editTemplate = (email: Object, index: Number) => {
  Form.value = { ...email };
  EamilIndex.value = index;
  showTemplateFields.value = true;
};
const updateData = async (data: Object) => {
  let options = {
    method: "Put",
    headers: {
      "Content-Type": "application/json",
      Accept: "application/json",
      Authorization:
        "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiYzljMGM3MjkzMWI0NGZjOWE1NTc5ZWMyOTg4NzVlYzMiLCJkIjoiMTY4MDA2MiIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMyNjczNzl9.3_zIDyeZ0ACxOF0VpywmxGpzdhUadzmvMRggb106s5E",
    },
    body: JSON.stringify(data),
  };
  const response = await useAuthLazyFetchPut(
    `https://v1-orm-lib.mars.hipso.cc/email-templates/${data.uid}`,
    options
  );
  emailTemplates.value[`${EamilIndex.value}`] = data;
};
</script>
