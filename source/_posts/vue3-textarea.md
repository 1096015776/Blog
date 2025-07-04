---
title: 封装一个输入框组件
date: 2025-07-04 10:00:00
tags:
  - Vue3
  - tip
---

# 需求
1. 回车发送消息
2. 支持换行
3. 通用


# 实现感悟

- 一个聊天输入框组件，看似简单，但从用户体验上来说,还是有很多细节需要注意的。
  - 回车发送消息(中文输入法拼写状态时不发送)
  - 支持换行(shift+回车,让浏览器默认执行，避免破坏系统撤销功能)
  - chrome浏览器自带bug,火狐yyds,末尾带换行符时，多次粘贴的情况光标乱跳(需要代码修复，提升用户体验,且粘贴时不能阻止浏览器默认行为,以免破坏系统撤销功能)

# 代码实现

```vue
<template>
  <textarea
    ref="textareaRef"
    class="w-full"
    :class="overclass"
    :style="{ height: height }"
    :placeholder="placeholder"
    v-model="inputValue"
    @keydown.enter="handleEnter"
    @compositionend="compositionend"
    @compositionstart="compositionstart"
    @paste="handlePaste"
  ></textarea>
</template>

<script lang="ts" setup>
withDefaults(
  defineProps<{
    height?: string
    overclass?: string
    placeholder?: string
  }>(),
  {
    height: 'unset',
    overclass: '',
    placeholder: ''
  }
)
const inputValue = defineModel<string>('modelValue')
const textareaRef = ref()
const emit = defineEmits(['enter'])

//处理中文输入法时回车字母发送消息问题
let inputEnd = true
const compositionend = (event: Event) => {
  inputEnd = true
}
const compositionstart = (event: Event) => {
  inputEnd = false
}

const handleEnter = (event: KeyboardEvent) => {
  if (inputEnd) {
    if (!event.shiftKey) {
      event.preventDefault()
      if (inputValue.value!.trim()) {
        emit('enter')
      }
    }
  }
}

// 处理当粘贴文本中存在换行符时，光标乱跳的问题
const handlePaste = (e: ClipboardEvent) => {
  const textarea = e.target as HTMLTextAreaElement
  const clipboardData = e.clipboardData || (window as any).clipboardData
  const pastedText = clipboardData?.getData('text') || ''
  const start = textarea.selectionStart
  setTimeout(() => {
    textarea.selectionStart = textarea.selectionEnd = start + pastedText.length
  }, 0)
}

const focus = () => {
  textareaRef.value.focus()
}

defineExpose({
  focus
})
</script>

```
