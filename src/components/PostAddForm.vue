<template>
	<div class="contents">
		<h1 class="page-header">Create Post</h1>
		<div class="form-wrapper">
			<form class="form" @submit.prevent="submitForm">
				<div>
					<label for="title">title</label>
					<input id="title" type="text" v-model="title" />
				</div>
				<div>
					<label for="content">contents</label>
					<textarea id="content" type="text" rows="5" v-model="contents" />
					<p v-if="isContentsValid" class="validation-text warning">
						Contents length must be less than 200
					</p>
				</div>
				<button type="submit" class="btn">Create</button>
			</form>
			<p class="log">
				{{ logMessage }}
			</p>
		</div>
	</div>
</template>

<script>
import { createPost } from '@/api/index';
export default {
	data() {
		return {
			// form values
			title: '',
			contents: '',
			// log
			logMessage: '',
		};
	},
	computed: {
		isContentsValid() {
			return this.contents.length >= 200;
		},
	},
	methods: {
		async submitForm() {
			try {
				const postData = {
					title: this.title,
					contents: this.contents,
				};
				const response = await createPost(postData);
				console.log(response);
				this.initForm();
			} catch (error) {
				console.log(error.response.data);
				this.logMessage = error.response.data.message;
			}
		},
		initForm() {
			this.title = '';
			this.contents = '';
		},
	},
};
</script>

<style scoped>
.form-wrapper .form {
	width: 100%;
}
.btn {
	color: white;
}
</style>
