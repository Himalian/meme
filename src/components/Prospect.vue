<script setup>
import { onMounted, ref, watch } from 'vue';
import Avatar from './Avatar.vue';

const STORAGE_KEY = 'characters';
const defaultAvatar = '/default-profile.png';
const characters = ref([
	{ name: '', description: '', avatar: defaultAvatar },
	{ name: '', description: '', avatar: defaultAvatar },
	{ name: '', description: '', avatar: defaultAvatar },
	{ name: '', description: '', avatar: defaultAvatar }
]);

onMounted(() => {
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
	<div class="grid grid-cols-2 grid-rows-2 items-start gap-4 w-fit p-4">
		<div v-for="(item, index) in characters" :key="index" class="flex flex-col items-center justify-center">
			<Avatar :src="item.avatar" @update:src="(val) => item.avatar = val" />
			<!-- <input v-model="item.name" placeholder=""
				class="font-bold mt-2 text-2xl w-full text-center border border-transparent focus:outline-none focus:border-b focus:border-black hover:border-zinc-600 break-all transition-colors">

			<input v-model="item.description" placeholder=""
				class="w-full text-center border border-transparent focus:outline-none focus:border-b focus:border-black hover:border-zinc-600 break-keep transition-colors"> -->
			<textarea
				class="w-full text-center text-2xl border border-transparent focus:outline-none focus:border-b focus:border-black hover:border-zinc-600 break-words transition-colors resize-none overflow-hidden"
				rows="1" style="min-height: 0; padding-top: 0; padding-bottom: 0;"
				oninput="this.style.height = ''; this.style.height = this.scrollHeight + 'px'"></textarea>
			<textarea
				class="w-full text-center border border-transparent focus:outline-none focus:border-b focus:border-black hover:border-zinc-600 break-words transition-colors resize-none overflow-hidden"
				rows="1" style="min-height: 0; padding-top: 0; padding-bottom: 0;"
				oninput="this.style.height = ''; this.style.height = this.scrollHeight + 'px'"></textarea>
		</div>
	</div>
</template>