# 如何优雅的二次封装组件

> 在我们的开发过程中，经常会遇到第三方组件无法满足需求的情况，因此会对组件进行二次封装。如何优雅的封装可读性高、可扩展性强的组件成为了前端开发者的必修课，本示例提供了vue框架二次封装组件的基本案例，如果对你有帮助，请推广给身边的同事、开发者。

### 错误的封装方式

假设现在需要对el-input进行二次封装，在输入框上方加一个标题

```vue
<script setup lang="ts">
import Ui1 from "./ui1.vue";
import {ref} from "vue";

const msg1 = ref('');
</script>

<template>
  <div class="second1">
    <ui1 v-model="msg1"></ui1>
  </div>
</template>

<style scoped>

</style>
```

```vue
<script setup lang="ts">
defineProps<{
  modelValue: string;
  title: string;
}>()

const emit = defineEmits<{
  (e: 'update:modelValue', value: string): void;
}>()
</script>

<template>
  <div>
    <div class="title">{{title}}</div>
    <el-input :model-value="modelValue" @input="emit('update:modelValue', $event)"></el-input>
  </div>
</template>

<style scoped>
.title {
  color: #c65b5b;
}
</style>
```

单单只是一个v-model就要怎么多代码量，假如还有其他的事件、属性呢？

```vue
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
    ></el-input>
  </div>
</template>

<style scoped>
.title {
  color: #c65b5b;
}
</style>
```

我仅仅只需要一个v-model，就给我封装了怎么多代码😨

如果还有插槽呢？

```vue
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
```

需求就在输入框上加个标题，这么麻烦😓

### 正确的封装方式

```vue
<script lang="ts" setup>
import {useAttrs, useSlots} from "vue";

defineProps<{ title: string }>()

const $attrs = useAttrs()
// console.log('attrs', $attrs)

const $slots = useSlots()
// console.log('slots', $slots)
</script>

<template>
  <div>
    <div class="title">{{ title }}</div>
    <el-input v-bind="$attrs">
      <template #[key] v-for="(slot, key) in $slots">
        <slot :name="key"></slot>
      </template>
    </el-input>
  </div>
</template>

<style scoped>
.title {
  color: #53853b;
}
</style>
```

几行代码就搞定了😊



[视频参考](https://b23.tv/fqF0QDG )

