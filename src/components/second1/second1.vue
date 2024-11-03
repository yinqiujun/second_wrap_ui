<script setup lang="ts">
import Ui1 from "./ui1.vue";
import {ref} from "vue";

const msg1 = ref('');
</script>

<template>
  <div class="second1">
    <ui1 v-model="msg1" title="这是错误的封装方式" placeholder="请输入">
      <template #prefix>
        <div>prefix</div>
      </template>
    </ui1>
  </div>
</template>

<style scoped>

</style>