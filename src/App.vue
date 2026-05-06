<script setup lang="ts">
import { useLocalStorage } from '@vueuse/core';
import mobile from 'is-mobile';
import mixpanel from 'mixpanel-browser';
import { provide, ref, toRaw } from 'vue';
import Prospect from './components/Prospect.vue';

</script>

<template>
  <div class="flex items-center w-120 mx-auto mb-2">
    <p class="mr-2">
      点击报告各部分即可填写～(∠・ω&lt; )⌒☆
    </p>
    <Button>
      保存为图片
    </Button>
  </div>

  <div
    class="w-120 h-165 mx-auto border border-black rounded-lg gap-4 flex flex-col"
    :style="{
    }"
  >
    <div class="flex justify-between">
      <div class="flex items-center">
        <h1 class="text-2xl font-bold">
			登场人物介绍
        </h1>
      </div>
    </div>

      <Prospect />
  </div>

</template>
