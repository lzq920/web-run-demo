<script setup lang="ts">
import Editor from './components/Editor.vue';
import Preview from './components/Preview.vue';
import { computed, ref } from 'vue';
const jsCode = ref('// javascript code');
const cssCode = ref('/* css code */');
const htmlCode = ref('<!-- html code -->');
const docCode = computed(() => {
  return {
    js: jsCode.value,
    css: cssCode.value,
    html: htmlCode.value,
  }
})
</script>

<template>
  <div class="grid">
    <div class="grid-item">
      <Editor v-model="jsCode" :readonly="false" language="javascript" />
    </div>
    <div class="grid-item">
      <Editor v-model="cssCode" :readonly="false" language="css" />
    </div>
    <div class="grid-item">
      <Editor v-model="htmlCode" :readonly="false" language="html" />
    </div>
    <div class="grid-item">
      <Preview :code="docCode" />
    </div>
  </div>
</template>

<style>
html,
body {
  margin: 0;
  padding: 0;
}

.grid {
  display: flex;
}

.grid-item {
  flex: 1;
}
</style>
