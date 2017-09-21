<template>
	<div class="editor" ref="editor">
		<pre class="markdown" v-html="markContent"></pre>
	</div>
</template>

<script>
import marked from 'marked';
export default {
	props: {
		resumeContent: String,
		isMarked: Boolean
	},
	computed: {
		markContent () {
			return this.isMarked ? marked(this.resumeContent) : this.resumeContent;
		}
	},
	methods: {
		goBottom () {
			this.$refs.editor.scrollTop = 10000;
		},
		goTop () {
			this.$refs.editor.scrollTop = 0;
		}
	}
};
</script>

<style lang="stylus">
.markdown
	height: 100%
	width: 100%
</style>
