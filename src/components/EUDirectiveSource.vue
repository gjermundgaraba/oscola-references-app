<script setup lang="ts">
import {NFormItem, NInput, NInputNumber, NButton, useNotification } from "naive-ui";
import type {NotificationType} from "naive-ui";
import {ref, reactive, watch} from "vue";
import Mustache from "mustache";

const model = reactive({
  organName: "",
  directiveNumber: "",
  title: "",
  year: 0,
  ojNumber: "",
})

const footnote = ref("");
const footnoteOutputHtml = ref<HTMLInputElement | null>(null);
const notification = useNotification()

watch(model, (newModel) => {
  footnote.value = Mustache.render("{{ organName }} Directive {{ directiveNumber }} {{ title }} [{{ year }}] OJ {{ ojNumber }}.", newModel)
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
  <n-form-item label="Organ name">
    <n-input v-model:value="model.organName" />
  </n-form-item>
  <n-form-item label="Directive number">
    <n-input v-model:value="model.directiveNumber" />
  </n-form-item>
  <n-form-item label="Title">
    <n-input v-model:value="model.title" />
  </n-form-item>
  <n-form-item label="Year">
    <n-input-number v-model:value="model.year" />
  </n-form-item>
  <n-form-item label="OJ number">
    <n-input v-model:value="model.ojNumber" />
  </n-form-item>
  <div @click="copyFootnote" ref="footnoteOutputHtml" v-html="footnote"></div>
</template>

<style scoped>

</style>
