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