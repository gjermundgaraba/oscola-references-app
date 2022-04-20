<script setup lang="ts">
import { darkTheme, NConfigProvider, NGlobalStyle, NNotificationProvider, NSelect } from "naive-ui";
import BookSource from "@/components/BookSource.vue";
import ContributionToEditedBookSource from "@/components/ContributionToEditedBookSource.vue"
import {ref} from "vue";
import JournalArticleSource from "@/components/JournalArticleSource.vue";
import TreatyInternationalSource from "@/components/TreatyInternationalSource.vue";
import JudgementECtHRSource from "@/components/JudgementECtHRSource.vue";
import EUDirectiveSource from "@/components/EUDirectiveSource.vue";
import JudgementCJEUSource from "@/components/JudgementCJEUSource.vue";

const sources = ref([
  { label: "Book", value: "BookSource" },
  { label: "Journal article", value: "JournalArticleSource" },
  { label: "Treaty - international", value: "TreatyInternationalSource" },
  { label: "Judgement ECtHR", value: "JudgementECtHRSource" },
  { label: "EU directive", value: "EUDirectiveSource" },
  { label: "Judgement CJEU", value: "JudgementCJEUSource" },
  /*{ label: "Contribution to edited book", value: "ContributionToEditedBookSource" },
  { label: "EU Commission document", value: "BookSource" },
  { label: "EU directive", value: "BookSource" },
  { label: "EU regulation", value: "BookSource" },
  { label: "ILC report", value: "BookSource" },

  { label: "Journal article (online)", value: "BookSource" },
  { label: "Judgement CJEU", value: "BookSource" },

  { label: "Judgement ICJ", value: "BookSource" },
  { label: "Newspaper article", value: "BookSource" },
  { label: "Other", value: "BookSource" },

  { label: "Treaty - regional", value: "BookSource" },
  { label: "UN document", value: "BookSource" },
  { label: "Website", value: "BookSource" },*/
]);

const selectedSource = ref();
const dt = darkTheme;

</script>

<template>
  <n-config-provider :theme="dt">
    <n-notification-provider>
      <main>
        <n-select v-model:value="selectedSource" :options="sources" />
        <BookSource v-if="selectedSource === 'BookSource'"></BookSource>
        <JournalArticleSource v-if="selectedSource === 'JournalArticleSource'"></JournalArticleSource>
        <TreatyInternationalSource v-if="selectedSource === 'TreatyInternationalSource'"></TreatyInternationalSource>
        <JudgementECtHRSource v-if="selectedSource === 'JudgementECtHRSource'"></JudgementECtHRSource>
        <EUDirectiveSource v-if="selectedSource === 'EUDirectiveSource'"></EUDirectiveSource>
        <JudgementCJEUSource v-if="selectedSource === 'JudgementCJEUSource'"></JudgementCJEUSource>
      </main>
      <footer>Made by <a href="https://github.com/bjaanes" target="_blank">Gjermund Garaba</a> - with ❤️</footer>
    </n-notification-provider>
    <n-global-style />
  </n-config-provider>
</template>

<style>
footer {
  text-align: center;
  position: fixed;
  bottom: 5px;
  width: 100%;
}

footer a {
  color: #fff;
}
</style>
