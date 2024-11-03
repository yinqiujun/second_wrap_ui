<script setup lang="ts">
import Ui2 from "./ui2.vue";
import {ref} from "vue";
import {Search} from '@element-plus/icons-vue'

const msg2 = ref('')
</script>

<template>
  <div class="second2">
    <ui2 v-model="msg2" title="这是正确的封装方式" placeholder="请输入">
      <template #prefix>
        <div>prefix</div>
      </template>
      <template #suffix>
        <el-icon>
          <search/>
        </el-icon>
      </template>
    </ui2>
  </div>
</template>

<style scoped>

</style>