<template>
	<v-container class="fill-height">
		<v-form ref="form" class="w-100 text-center">
			<v-file-input v-model="file" label="选择文件"></v-file-input>
			<div class="mx-auto">
				<v-btn class="mr-4" @click="uploadFile">上传文件</v-btn>
				<v-btn @click="count++"> count: {{ count }}</v-btn>
			</div>
		</v-form>
		<v-divider class="my-5" />
		<v-card class="mx-auto" width="300">
			<v-list :items="items"></v-list>
		</v-card>
	</v-container>
</template>

<script setup lang="ts">
import { onMounted, ref, unref } from 'vue'
import axios from 'axios'

const file = ref()
const count = ref(0)
const items = ref([])

function uploadFile() {
	console.log(unref(file))
	const formData = new FormData()
	formData.set('file', unref(file)[0])
	axios.post('http://localhost:3000/upload', formData).then(res => {
		console.log(res)
	})
}

onMounted(() => {
	axios.get('http://localhost:3000/user').then(res => {
		items.value = res.data.data.map(item => {
			return {
				title: item.username,
				value: item.id
			}
		})
	})
})
</script>
