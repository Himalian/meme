<script lang="ts" setup>
import defaultAvatar from '/default-profile.png'
import { ref, useTemplateRef } from 'vue';
import 'vue-cropper/dist/index.css'
import { VueCropper } from 'vue-cropper'

const props = defineProps<{
	src?: string
}>()

const emit = defineEmits<{
	'update:src': [value: string]
}>()

const editAvatar = ref(false)
const cropperImg = ref<string | null>(null)
const cropperRef = useTemplateRef<InstanceType<typeof VueCropper>>('cropper')
const fileInput = useTemplateRef<HTMLInputElement>('fileInput')

function toggleEditAvatar() {
	editAvatar.value = !editAvatar.value
	if (editAvatar.value && props.src) {
		cropperImg.value = props.src
	}
	if (!editAvatar.value) {
		cropperImg.value = null
	}
}

function handleFileChange(e: Event) {
	const file = (e.target as HTMLInputElement).files?.[0]
	if (!file) return

	const reader = new FileReader()
	reader.onload = (event) => {
		cropperImg.value = event.target?.result as string
	}
	reader.readAsDataURL(file)
}

function onConfirm() {
	cropperRef.value?.getCropData((data: string) => {
		emit('update:src', data)
		editAvatar.value = false
		cropperImg.value = null
	})
}

function onCancel() {
	editAvatar.value = false
	cropperImg.value = null
}
</script>

<template>
	<div @click="toggleEditAvatar" class="hover:cursor-pointer">
		<img 
		:src="props.src || defaultAvatar" 
		alt="Default Avatar"
		class="border border-black rounded-full"
		 />
	</div>
	<Teleport to="body">
		<div v-if="editAvatar" class="fixed inset-0 z-50 flex items-center justify-center bg-black/50">
			<div class="border bg-white border-black rounded-lg flex flex-col w-96 p-4">
				<p class="text-2xl font-bold mb-4">Update Avatar</p>

				<div class="flex flex-col gap-4">
					<div v-if="cropperImg" class="w-full h-64 bg-zinc-100 rounded overflow-hidden">
						<VueCropper
							ref="cropper"
							:img="cropperImg"
							:autoCrop="true"
							:fixed="true"
							:fixedNumber="[1, 1]"
							outputType="png"
						/>
					</div>
					<div v-else class="w-full h-64 bg-zinc-100 rounded flex items-center justify-center border-2 border-dashed border-zinc-300">
						<p class="text-zinc-500">No image selected</p>
					</div>

					<input
						type="file"
						ref="fileInput"
						class="hidden"
						accept="image/*"
						@change="handleFileChange"
					/>
					
					<button 
						@click="fileInput?.click()"
						class="w-full py-2 bg-zinc-100 hover:bg-zinc-200 rounded border border-zinc-300 transition-colors"
					>
						Select Image
					</button>
				</div>

				<div class="flex gap-2 flex-row-reverse mt-6">
					<button
						class="px-4 py-2 rounded-lg bg-zinc-800 text-white hover:bg-zinc-900 transition-colors"
						@click="onConfirm"
						:disabled="!cropperImg"
					>
						Confirm
					</button>
					<button
						class="px-4 py-2 rounded-lg border border-zinc-300 hover:bg-zinc-100 transition-colors"
						@click="onCancel"
					>
						Cancel
					</button>
				</div>
			</div>
		</div>
	</Teleport>
</template>
<style scoped>
/* 深度作用选择器，确保样式能渗透进 VueCropper 组件内部 */
:deep(.cropper-crop-box img),
:deep(.cropper-view-box img),
:deep(.cropper-canvas img) {
  max-width: none !important;
  height: auto !important;
}
</style>
