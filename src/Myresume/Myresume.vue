<template>
	<div class="resume" ref="resumeContent">
		<pre v-html="codeInContent"></pre>
		<div v-html="codeInStyles"></div>
	</div>
</template>

<script>
import prism from 'prismjs';
export default {
	props: {
		styleContent: String
	},
	computed: {
		codeInStyles () {
			return `<style>${this.styleContent}</style>`;
		},
		codeInContent () {
      return prism.highlight(this.styleContent, prism.languages.css)
    },
	},
	methods: {
		goBottom () {
			this.$refs.resumeContent.scrollTop = 10000;
		}
	}
};
</script>

<style lang="stylus">
.resume
	font-size: 14px
</style>
