<script setup lang="ts">
import { onMounted, defineProps, defineEmits, ref, watch, onBeforeUnmount } from 'vue';
import * as monaco from 'monaco-editor';
import editorWorker from 'monaco-editor/esm/vs/editor/editor.worker?worker'
import jsonWorker from 'monaco-editor/esm/vs/language/json/json.worker?worker'
import cssWorker from 'monaco-editor/esm/vs/language/css/css.worker?worker'
import htmlWorker from 'monaco-editor/esm/vs/language/html/html.worker?worker'
import tsWorker from 'monaco-editor/esm/vs/language/typescript/ts.worker?worker'
self.MonacoEnvironment = {
    getWorker(workerId: string, label: string) {
        if (label === 'json') {
            return new jsonWorker()
        }
        if (label === 'css' || label === 'scss' || label === 'less') {
            return new cssWorker()
        }
        if (label === 'html' || label === 'handlebars' || label === 'razor') {
            return new htmlWorker()
        }
        if (label === 'typescript' || label === 'javascript') {
            return new tsWorker()
        }
        return new editorWorker()
    }
}
interface Props {
    modelValue: string;
    language: string;
    readonly: boolean;
}
const props = withDefaults(defineProps<Props>(), {
    modelValue: '// Type your code here',
    language: 'javascript',
    readonly: false,
});
const emit = defineEmits(['update:modelValue']);
const dom = ref();
let instance: monaco.editor.IStandaloneCodeEditor;
onMounted(() => {
    instance = monaco.editor.create(dom.value, {
        value: props.modelValue,
        language: props.language,
        theme: 'vs-dark',
        minimap: {
            enabled: false
        },
        readOnly: props.readonly,
    });
    // 监听内容变化
    instance.onDidChangeModelContent(() => {
        const value = instance.getValue();
        emit('update:modelValue', value);
    });
    // 监听键盘事件
    instance.addCommand(monaco.KeyMod.CtrlCmd | monaco.KeyCode.KeyS, () => {
        emit('update:modelValue', instance.getValue());
    });
    // 添加保存菜单
    instance.addAction({
        id: 'save',
        label: 'Save',
        keybindings: [monaco.KeyMod.CtrlCmd | monaco.KeyCode.KeyS],
        contextMenuGroupId: '9_cutcopypaste',
        contextMenuOrder: 1.5,
        run: () => {
            emit('update:modelValue', instance.getValue());
        }
    });
});
onBeforeUnmount(() => {
    instance.dispose();
})
watch(() => props.modelValue, () => {
    if (props.readonly) {
        instance.setValue(props.modelValue);
    }
});
</script>

<template>
    <div ref="dom" class="editor"></div>
</template>
<style scoped>
.editor {
    height: 100vh;
}
</style>