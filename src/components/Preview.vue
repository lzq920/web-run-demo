<script setup lang="ts">
import { defineProps } from 'vue';
interface Props {
    code: any;
}
const props = withDefaults(defineProps<Props>(), {
    code: {
        js: '',
        css: '',
        html: '',
    },
});
const renderDoc = (code: any) => {
    return `
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Preview</title>
        <style>
            ${code.css}
        <\/style>
    </head>
    <body>
        ${code.html}
        <script>
        ${code.js}
        <\/script>
    <\/body>
    <\/html>
    `
}
</script>
<template>
    <div class="preview">
        <iframe :srcdoc="renderDoc(code)"></iframe>
    </div>
</template>
<style scoped>
.preview {
    height: 100vh;
    background-color: #1e1e1e;
}

iframe {
    border: none;
    width: 100%;
    height: 100vh;
}
</style>