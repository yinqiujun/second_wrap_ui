<script setup lang="ts">

defineProps<{
  modelValue: string;
  min?: number;
  max?: number;
  placeholder?: string;
  label?: string;
  type?: string;
  clearable?: boolean;
  showPassword?: boolean;
  disabled?: boolean;
  readonly?: boolean;
  autofocus?: boolean;
  autocomplete?: string;
  step?: number;
  title?: string;
  // ...
}>()

const emit = defineEmits<{
  (e: 'update:modelValue', value: string): void;
  (e: 'change', value: string): void;
  (e: 'focus', value: string): void;
  (e: 'blur', value: string): void;
  (e: 'clear'): void;
  (e: 'mouseleave'): void;
  (e: 'mouseenter'): void;
  (e: 'keydown'): void;
  (e: 'keyup'): void;
  (e: 'compositionstart'): void;
  (e: 'compositionend'): void;
  (e: 'paste'): void;
  // ...
}>()


</script>

<template>
  <div>
    <div class="title">{{ title }}</div>
    <el-input
        :model-value="modelValue"
        :min="min"
        :max="max"
        :placeholder="placeholder"
        :label="label"
        :type="type"
        :clearable="clearable"
        :show-password="showPassword"
        :disabled="disabled"
        :readonly="readonly"
        :autofocus="autofocus"
        :autocomplete="autocomplete"
        :step="step"
        @input="emit('update:modelValue', $event)"
        @change="emit('change', $event)"
        @focus="emit('focus', $event)"
        @blur="emit('blur', $event)"
        @clear="emit('clear')"
        @mouseleave="emit('mouseleave')"
        @mouseenter="emit('mouseenter')"
        @keydown="emit('keydown')"
        @keyup="emit('keyup')"
        @compositionstart="emit('compositionstart')"
        @compositionend="emit('compositionend')"
        @paste="emit('paste')"
    >
      <template #prefix>
        <slot name="prefix"></slot>
      </template>
      <!--<template #suffix>-->
      <!--  <slot name="suffix"></slot>-->
      <!--</template>-->
      <!--<template #prepend>-->
      <!--  <slot name="prepend"></slot>-->
      <!--</template>-->
      <!--<template #append>-->
      <!--  <slot name="append"></slot>-->
      <!--</template>-->
    </el-input>
  </div>
</template>

<style scoped>
.title {
  color: #c65b5b;
}
</style>