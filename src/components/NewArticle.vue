<template>
	<div class="newarticle container">
		<div class="loader" v-if="loading"></div>
		<div class="main" v-if="!loading">
			<form v-on:submit.prevent="publishArticle">
				<div class="form-group">
					<label for="articleTitle">Article Title</label>
					<input type="text" class="form-control" id="articleTitle" placeholder="Article Title" v-model="article.title">
				</div>
				<div class="form-group">
					<label for="articleAbout">What's this article about?</label>
					<input type="text" class="form-control" id="articleAbout" placeholder="Article Description" v-model="article.description">
				</div>
				<div class="form-group">
					<label for="articleBody">Article Body</label>
					<textarea class="form-control" id="articleBody" rows="6" placeholder="Write your article (in markdown)" v-model="article.body"></textarea>
				</div>
				<div class="form-group">
					<label for="articleTags">Article Tags</label>
					<input type="text" class="form-control" id="articleTags" placeholder="Enter Tags (separated by comma',')" v-model="article.tagList">
				</div>

				<div class="text-center">
					<input type="submit" class="btn btn-success" value="Publish Container">
				</div>
			</form>
		</div>
	</div>
</template>

<script>
import ArticleService from "@/services/ArticleService";

export default {
	name: 'newarticle',
	data: () => { return {
		loading: false,
		article: {
			title: '',
			description: '',
			body: '',
			tagList: '',
		},
		modeEnum: {
			add: 0,
			edit: 1
		},
		currentMode: 0
	}},
	props: {
		edit: Boolean
	},
	methods: {
		async getArticle(slug) {
			const response = await ArticleService.getArticle(slug);
			if (response && response.data && response.data.article) {
				return response.data.article;
			}
			else return null;
		},
		async publishArticle() {
			this.loading = true;

			let data = {
				title: this.article.title,
				description: this.article.description,
				body: this.article.body,
				tagList: this.article.tagList.toString().split(',')
			}

			let response = (this.edit)? ArticleService.updateArticle(this.slug, data) : ArticleService.addArticle(data);
			response = await response;
			if (response && response.data && response.data.article) {
				this.$router.push('/article/'+response.data.article.slug);
			}
			this.loading = false;
		}
	},
	computed: {
		slug() {
			return this.$route.params.slug;
		},
		user() {
			return this.$root.user;
		},
		isOwner() {
			if (this.$root.user && this.$root.user.username == this.article.author.username) {
				return true;
			}
			return false;
		}
	},
	async created() {
		if (this.edit) {
			this.loading = true;

			console.log(this.$root.user);
			const articleData = await this.getArticle(this.slug);
			if (this.user.username != articleData.author.username) {
				alert('Must be your post to edit');
				this.$router.push('/');
				return;
			}
			this.article = articleData;
			this.loading = false;
		}
	}
}
</script>
