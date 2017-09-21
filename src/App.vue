<template>
  <div id="app">
    <myresume :styleContent="currentStyleContent" ref="resume"></myresume>
    <resume-content :isMarked="isMarked" :resumeContent="currentResumeContent" ref="resumeContent"></resume-content>
  </div>
</template>

<script>
import resumeContent from './ResumeContent/ResumeContent.vue';
import myresume from './Myresume/Myresume.vue';
export default {
  name: 'app',
  data () {
    return {
      currentStyleContent: '',
      styleContent:
`
* 原出处 http://strml.net/
* 大家好，我叫陈东奇
* 秋招开始了，我一个offer都没拿到
* 希望做这个简历能给我点luck吧
* 废话不多说我们开始写吧
*/
/* 样式有点丑，我们先给页面加点样式 */
* {
  transition: all .3s;
}
/* 白色背景太单调了，我们来点背景 */
html {
  color: rgb(222,222,222);
  background: rgb(0,43,54);
}
/* 文字离边框太近了 */
.resume {
  padding: .5em;
  border: 1px solid;
  overflow: auto;
  width: 90vw;
  margin: 2.5vh 5vw;
  height: 80vh;
}
/* 代码高亮一下 */
.token.selector{
  color: #CBBA7D;
}
.token.property{
  color: #98D0E9;
}
.token.punctuation{
  color: #fff;
}
.token.function{
  color: #CE773F;
}
/* 加点 3D 效果呗 */
html{
  perspective: 1000px;
}
/* 立体一点，留点位置给简历吧 */
.resume {
  position: relative; 
  left: 0; top: 0;
  width: 40vw;
  transform: rotateY(30deg);
}
/* 小屏幕这样就不好看了，注意一下响应式 */
@media screen and (max-width: 620px) {
  .resume {
    width: 90vw;
    height: 40vh;
    transform: rotateX(-30deg);
  }
}
/* 接下来我给自己准备一个编辑器 */
.editor{
  box-sizing: border-box;
  position: fixed;
  top: 0;
  right: 0;
  padding: .5em;
  margin: 2.5vh;
  width: 50vw;
  height: 85vh;
  border: 1px solid;
  background: white; 
  color: #222;
  overflow: auto;
}
/* 屏幕小于620px的时候 */
@media screen and (max-width: 620px) {
  .editor {
    top: 50%;
    width: 90vw;
    height: 40vh;
  }
}
/* 好了我要开始写简历了 */

`,
    afterStyleContent:
`
/* 显然我用的是markdown格式
 * 现在我们把简历转化为html格式
 * 用了一下开源工具marked
 */
/* 对 HTML 加点样式好看一点 */
.editor{
  padding: 1em;
  font-size: 0.8em;
}
.editor h2{
  display: inline-block;
  font-size: 1.5em;
  margin: 0.5em;
}
.editor ul,.editor ol{
  padding: 0;
  list-style: none;
}
.editor ul> li::before{
  content: '•';
  margin-right: 0.5em;
}
.editor ol {
  counter-reset: section;
}
.editor ol li::before {
  counter-increment: section;
  content: counters(section, ".") " ";
  margin-right: .5em;
}
.editor blockquote {
  padding: .5em;
  background: #ddd;
}
/* 好了我写完了，谢谢观看 */
`,
    currentResumeContent: '',
    // 是否已经marked转换
    isMarked: false,
    resumeContent: 
`
## 陈东奇
广东财经大学，想找份前端开发岗位工作

## 技能
* vue2.0
* react
* Node.js

## 项目经验
* 十牛公司微信版客户端
* 个人单页博客(vue+node+mysql)

## 校内经验
* 班级班长
* 院学生会干事
* 国旗队升旗手

## 实习经历
* 十牛信息科技有限公司

## 兴趣爱好
1. 健身跑步
2. 音乐电影
3. 新技术

## 希望笔试面试过过过

链接
----
> [GitHub](https://github.com/KnightKiz)`,
    speed: 50
    }
  },
  created () {
    this._beginning();
  },
  methods: {
    _styleEditor () {
			return new Promise((resolve, reject) => {
				let n = 0;
				let timer = window.setInterval(() => {
          this.currentStyleContent = this.styleContent.substring(0, n);
          this.$nextTick(() => {
            this.$refs.resume.goBottom();
          });
					n++;
					if (n === this.styleContent.length) {
            window.clearInterval(timer);
						resolve();
					}
				}, this.speed);
      });
    },
    _contentEditor () {
      return new Promise((resolve, reject) => {
        let n = 0;
        let timer = window.setInterval(() => {
          this.currentResumeContent = this.resumeContent.substring(0, n);
          n++;
          this.$nextTick(() => {
            this.$refs.resumeContent.goBottom();
          });
          if (n === this.resumeContent.length) {
            window.clearInterval(timer);
						resolve();
					}
        }, this.speed);
      });
    },
    _afterContentEditor () {
      return new Promise((resolve, reject) => {
        let n = 0;
        let temStr = '';
				let timer = window.setInterval(() => {
          if (n === 50) {
            this.isMarked = true;
          }
          temStr = this.afterStyleContent.substring(n, n + 1);
          this.currentStyleContent = this.currentStyleContent + temStr;
          this.$nextTick(() => {
            this.$refs.resume.goBottom();
          });
					n++;
					if (n === this.afterStyleContent.length) {
            window.clearInterval(timer);
						resolve();
					}
				}, this.speed);
      });
    },
		async _beginning () {
      await this._styleEditor();
      await this._contentEditor();
      await this._afterContentEditor();
		}
  },
  components: {
    myresume,
    resumeContent
  }
}
</script>

<style lang="stylus">
* 
  transition: all .3s
#app
  position: relative
  height: 100%
  width: 100%
</style>
