<script setup lang="ts">
import {NFormItem, NInput, NInputNumber, NButton, useNotification } from "naive-ui";
import type {NotificationType} from "naive-ui";
import {ref, reactive, watch} from "vue";
import Mustache from "mustache";

const model = reactive({
  titleTreaty: "",
  adopted: "",
  enteredIntoForce: "",
  treatySeries: "",
  firstPage: 0,
  shortTitle: ""
})

const footnote = ref("");
const footnoteOutputHtml = ref<HTMLInputElement | null>(null);
const notification = useNotification()

watch(model, (newModel) => {
  footnote.value = Mustache.render("{{ titleTreaty }} ({{ adopted }}, {{ enteredIntoForce }}) {{ treatySeries }} {{ firstPage }} ({{ shortTitle }}).", newModel)
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
  <n-form-item label="Title treaty">
    <n-input v-model:value="model.titleTreaty" />
  </n-form-item>

  <n-form-item label="Adopted">
    <n-input v-model:value="model.adopted" />
  </n-form-item>

  <n-form-item label="Entered into force">
    <n-input v-model:value="model.enteredIntoForce" />
  </n-form-item>

  <n-form-item label="Treaty series">
    <n-input v-model:value="model.treatySeries" />
  </n-form-item>

  <n-form-item label="First page">
    <n-input-number v-model:value="model.firstPage" />
  </n-form-item>

  <n-form-item label="Short title">
    <n-input v-model:value="model.shortTitle" />
  </n-form-item>

  <div @click="copyFootnote" ref="footnoteOutputHtml" v-html="footnote"></div>
</template>

<style scoped>

</style>
