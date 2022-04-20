<script setup lang="ts">
import {NFormItem, NInput, NInputNumber, NButton, useNotification } from "naive-ui";
import type {NotificationType} from "naive-ui";
import {ref, reactive, watch} from "vue";
import Mustache from "mustache";

const model = reactive({
  caseNumber: "",
  caseName: "",
  year: 0,
  reportNumber: "",
  firstPage: 0,
})

const footnote = ref("");
const footnoteOutputHtml = ref<HTMLInputElement | null>(null);
const notification = useNotification()

watch(model, (newModel) => {
  footnote.value = Mustache.render("Case {{ caseNumber }} <i>{{ caseName }}</i> [{{ year }}] {{ reportNumber }} {{ firstPage }}.", newModel)
})

function copyFootnote() {
  if (footnoteOutputHtml.value) {
    copyToClip(footnoteOutputHtml.value.innerHTML);
    notification.info({
      content: 'Footnote copied to clipboard',
      duration: 1500,
    })
  }
}

function copyToClip(str: string) {
  function listener(e: ClipboardEvent) {
    if (e.clipboardData === null) return;
    e.clipboardData.setData("text/html", str);
    e.clipboardData.setData("text/plain", str);
    e.preventDefault();
  }
  document.addEventListener("copy", listener);
  document.execCommand("copy");
  document.removeEventListener("copy", listener);
};
</script>

<template>

  <n-form-item label="Case number">
    <n-input v-model:value="model.caseNumber" />
  </n-form-item>
  <n-form-item label="Case name">
    <n-input v-model:value="model.caseName" />
  </n-form-item>
  <n-form-item label="Year">
    <n-input-number v-model:value="model.year" />
  </n-form-item>
  <n-form-item label="Report number">
    <n-input v-model:value="model.reportNumber" />
  </n-form-item>
  <n-form-item label="First page">
    <n-input-number v-model:value="model.firstPage" />
  </n-form-item>
  <div @click="copyFootnote" ref="footnoteOutputHtml" v-html="footnote"></div>
</template>

<style scoped>

</style>
