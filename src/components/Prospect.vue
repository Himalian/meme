<script setup>
import { onMounted, ref, watch } from 'vue';
import Avatar from './Avatar.vue';

const STORAGE_KEY = 'characters';
// 定义四个格子的初始数据
const characters = ref([
  { name: '', description: '', avatar: '/src/assets/default-profile.png' },
  { name: '', description: '', avatar: '/src/assets/default-profile.png' },
  { name: '', description: '', avatar: '/src/assets/default-profile.png' },
  { name: '', description: '', avatar: '/src/assets/default-profile.png' }
]);

onMounted(()=>{
	const saved = localStorage.getItem(STORAGE_KEY);

	if (saved) {
		characters.value = JSON.parse(saved)
	}
})

watch(characters, (newVal) => {
	localStorage.setItem(STORAGE_KEY, JSON.stringify(newVal))
}, { deep: true })
</script>

<template>
  <div class="grid grid-cols-2 grid-rows-2 gap-4 w-fit p-4">
    <div 
      v-for="(item, index) in characters" 
      :key="index"
      class="flex flex-col items-center justify-center"
    >
      <!-- 头像部分 -->
      <Avatar :src="item.avatar" @update:src="(val) => item.avatar = val" />
      <!-- 输入部分：双向绑定到数组项 -->
      <input 
        v-model="item.name"
        placeholder=""
        class="font-bold text-2xl w-full text-center border-none focus:border-b focus:border-black break-all"
      >
	  <input 
		v-model="item.description"
		placeholder=""
		class="max-w-full text-center border-none focus:border-b focus:border-black break-all"
	  >
    </div>
  </div>
</template>