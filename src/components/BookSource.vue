<script setup lang="ts">
import {NFormItem, NInput, NInputNumber, NButton, useNotification } from "naive-ui";
import type {NotificationType} from "naive-ui";
import {ref, reactive, watch} from "vue";
import Mustache from "mustache";

const createAuthor = () => ({
  firstName: "",
  lastName: "",
  firstNameInitials() {
    return this.firstName.split(' ').map(fn => fn[0]).join('');
  }
})

const model = reactive({
  title: "",
  edition: "",
  publisher: "",
  year: 0,
  authors: [
    createAuthor()
  ],
  editionLowercase() {
    if (this.edition) {
      return this.edition.toLowerCase();
    }

  },
})

const footnote = ref("");
const bibliography = ref("");
const footnoteOutputHtml = ref<HTMLInputElement | null>(null);
const bibliographyOutputHtml = ref<HTMLInputElement | null>(null);
const notification = useNotification()

function addAuthor() {
  model.authors.push(createAuthor());
}

watch(model, (newModel) => {
  footnote.value = Mustache.render("{{ #authors }}{{ firstName }} {{ lastName }}, {{ /authors }}<i>{{ title }}</i> ({{#editionLowercase}}{{ . }} edn, {{/editionLowercase}}{{ publisher }} {{ year }}).", newModel)
  bibliography.value = Mustache.render("{{ #authors }}{{ lastName }} {{ firstNameInitials }}, {{ /authors }}<i>{{ title }}</i> ({{#editionLowercase}}{{ . }} edn, {{/editionLowercase}}{{ publisher }} {{ year }})", newModel)
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

function copyBibliography() {
  if (bibliographyOutputHtml.value) {
    copyToClip(bibliographyOutputHtml.value.innerHTML);
    notification.info({
      content: 'Bibliography copied to clipboard',
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
  <div v-for="(author, index) in model.authors" :key="index">
    <n-form-item :label="`${index + 1} Author firstname`">
      <n-input v-model:value="author.firstName" />
    </n-form-item>
    <n-form-item :label="`${index + 1} Author lastname`">
      <n-input v-model:value="author.lastName" />
    </n-form-item>
  </div>
  <n-button @click="addAuthor">Add author</n-button>
  <n-form-item label="Title">
    <n-input v-model:value="model.title" />
  </n-form-item>
  <n-form-item label="Edition">
    <n-input v-model:value="model.edition" />
  </n-form-item>
  <n-form-item label="Publisher">
    <n-input v-model:value="model.publisher" />
  </n-form-item>
  <n-form-item label="Year">
    <n-input-number v-model:value="model.year" />
  </n-form-item>
  <div @click="copyFootnote" ref="footnoteOutputHtml" v-html="footnote"></div>
  <div @click="copyBibliography" ref="bibliographyOutputHtml" v-html="bibliography"></div>
</template>

<style scoped>

</style>
