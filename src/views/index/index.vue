<template>
    <div class="blogBox">
        <!--  -->
        <pre ref='mainBoxs' id='mainBoxs' v-html='fullTextObj.mainBoxs'></pre>
        <pre ref='workBoxs' id='workBoxs' v-html='fullTextObj.workBoxs' v-if='!showTureWork'></pre>
        <div ref='t_workBoxs' id='templateBoxs' v-else>
          <div class="blogCon">
            <div class="header">
              <div class="h_title">Yezannnn's Web Blog</div>
              <p class="h_p_title">
                生而为人,我很庆幸
                <br/>
                <br/>
                <a @click='jumpToCV'>🎁宝藏</a>
              </p>
              <div class="toollist">
                <ul>
                  <li @click='shareIcon(1)'><img src="@/assets/social_google.png"></li>
                  <li @click='shareIcon(2)'><img src="@/assets/social_github.png"></li>
                  <li @click='shareIcon(3)'><img src="@/assets/social_wechat.png"></li>
                </ul>
              </div>
            </div>
            <div class="content">
              <div class="c_title">🔥 yezannnnn的前端技术博客,每周一至周五不定时更新 🔥</div>
              <ul class='c_con'>
                <li v-for='item in myBlogList'>
                  <!-- <div :class="{ showJs : item.type === 1 , showVue : item.type === 2 ,showGo : item.type === 3 ,showPhp : item.type === 4}"></div> -->
                  <div class="text">
                    <img v-if='item.type === 1' src="@/assets/js.png">
                    <img v-if='item.type === 2' src="@/assets/logo.png">
                    <img v-if='item.type === 3' src="@/assets/golang.png">
                    <img v-if='item.type === 4' src="@/assets/PHP.png">
                    <h1>{{ item.title }}</h1>
                  </div>
                  <div class="date">{{ item.date }}</div>
                </li>
              </ul>
            </div>
            <div class='footer'>
              <div>博客内容：yezannnnn@sina.com 设计与灵感来自 <a href="www.STRML.net">STRML.net</a></div>
            </div>
          </div>
        </div>

        <div v-if='showTureWork' class="flexBox">👾</div>
    </div>
</template>
<!-- <style id="style-tag"></style> -->
<!-- <style ref='styleBoxs' id='styleBoxs'></style> -->
<script>
// import HelloWorld from './components/HelloWorld.vue'
// import { VueTypedJs } from 'vue-typed-js'
// 读取字符串 raw-loader
import styleCss0 from 'raw-loader!@/styles/main0.txt'
import styleCss1 from 'raw-loader!@/styles/main1.txt'
import workTxt from 'raw-loader!@/styles/work.txt'
// import styleCss2 from 'raw-loader!@/styles/main0.txt'
import Cookies from 'js-cookie'
import Promise from 'bluebird';
// import indexTxt from 'raw-loader!@/views/index/index.txt'
export default {
    name: 'index',
    components: {
        // VueTypedJs
    },

    data() {
        return {
            // 缓存目前样式已经存入的str
            // 对象 存储css str及html str
            fullTextObj: {
                "mainBoxs": '',
                "workBoxs": '',
                "otherBoxs": '',
            },
            styleBufferObj: {
                "mainBoxs": '',
                "workBoxs": '',
                "otherBoxs": '',
            },
            showTureWork: false,
            mainBoxs: null,
            styleBoxs: null,
            speed: 12,
            styleMaps: null,
            endOfSentence: /[\.\?\!]\s$/,
            comma: /\D[\,]\s$/,
            endOfBlock: /[^\/]\n\n$/,
            // 匹配 注释
            commentRegex: /(\/\*(?:[^](?!\/\*))*\*)$/,
            // 
            keyRegex: /([a-zA-Z- ^\n]*)$/,
            // 匹配 css的赋值
            valueRegex: /([^:]*)$/,
            // 匹配 class样式及选择器
            selectorRegex: /(.*)$/,
            // 匹配 单位px及dp
            pxRegex: /\dp/,
            pxRegex2: /p$/,
            // 是否注释开始了
            openComment: false,
            paused: false,
            // type 1 JS ,2Vue,3Golang,4PHP
            myBlogList: [{ title: '前端劝退预警：JavaScript 工具链不完全指南',type:1,date:'2020-01-02' }, { title: '前端code',type:2,date:'2020-01-02' }, { title: '前端code',type:3,date:'2020-01-02' }, { title: '前端code',type:4,date:'2020-01-02' }, { title: '前端code',type:1 ,date:'2020-01-02'}, { title: '前端code',type:1 ,date:'2020-01-02'}, ],
        }
    },
    created() {
        var self = this
        this.checkCookies()
        setTimeout(function() {
            self.getEls()
            self.startA()

        }, 1000)

    },
    mounted() {},
    methods: {
        async getEls() {
            this.mainBoxs = this.$refs.mainBoxs
            this.workBoxs = this.$refs.workBoxs
            // style 在Vue中取不到 写在index.html
            this.styleBoxs = document.getElementById("styleBoxs")
        },
        async startA() {
            var self = this

            await self.codeInto(self.mainBoxs, 0, styleCss0, self.speed, 1, true, self.styleBoxs)
            await self.codeInto(self.workBoxs, 0, workTxt, self.speed - 12, 1, false, self.styleBoxs)
            await self.codeInto(self.mainBoxs, 0, styleCss1, self.speed, 1, true, self.styleBoxs)

            // 调试样式用
            // console.log(styleCss0 + "\n" +styleCss1)
            // var allstyle = styleCss0 + "\n" +styleCss1
            // self.mainBoxs.innerHTML = allstyle
            // self.styleBoxs.innerHTML = allstyle
            // this.showTureWork = true

        },
        async codeInto(el, index, str, speed, charSpeed, mirrorToStyle, style) {
            var self = this

            if(this.skipIt) {
              throw new Error('SKIP IT');
            }

            let char = str.slice(index, index + charSpeed)
            if (mirrorToStyle) {
                self.codeChar(el, char, style)
            } else {
                self.codeSimpleChar(el, char)
            }
            // 一直居于底部
            el.scrollTop = el.scrollHeight;

            index += charSpeed;
            if (index < str.length) {
                let thisInterval = speed
                let slices = str.slice(index - 2, index + 1)

                if (this.comma.test(slices)) thisInterval = self.speed * 30
                if (this.endOfBlock.test(slices)) thisInterval = self.speed * 50
                if (this.endOfSentence.test(slices)) thisInterval = self.speed * 70

                do {
                    // 一定要用Promise 
                    await Promise.delay(thisInterval);
                } while (this.paused)

                return self.codeInto(el, index, str, speed, charSpeed, mirrorToStyle, style)
            } else {
                if (el.id === 'workBoxs') this.showTureWork = true
            }
        },
        codeChar(el, str, styleel) {
            let fullText = ''

            // 给box添加内容
            try {
                fullText = this.fullTextObj[el.id]
                fullText = this.replaceChar(fullText, str)
                this.fullTextObj[el.id] = fullText
            } catch (err) {
                console.log(err)
            }

            // 给样式添加样式
            let styleBuffer = this.styleBufferObj[el.id]
            styleBuffer += str

            if (str === ';') {
                styleel.textContent += styleBuffer;
                styleBuffer = '';
            }
            this.styleBufferObj[el.id] = styleBuffer

        },
        codeSimpleChar(el, str) {
            el.innerHTML += str
        },

        replaceChar(fullText, str) {
            if (str !== '/' && this.openComment) {
                // 第一次判断是否在注释中的str
                fullText += str
            } else if (str === '/' && !this.openComment) {
                // 判断是否注释未开始
                this.openComment = true;
                fullText += str
            } else if (str === '/' && fullText.slice(-1) === '*' && this.openComment === true) {
                // 判断注释中的str是不是结束
                this.openComment = false
                // 这里呢 匹配到结束注释才会套上span标签
                fullText = fullText.replace(this.commentRegex, '<span class="comment">$1/</span>')
            } else if (str === ':') {
                // 匹配样式的名称
                fullText = fullText.replace(this.keyRegex, '<span class="key">$1</span>:');
            } else if (str === ';') {
                // 匹配样式名称对应的key
                fullText = fullText.replace(this.valueRegex, '<span class="value">$1</span>;');
            } else if (str === '{') {
                // 匹配 选择器
                fullText = fullText.replace(this.selectorRegex, '<span class="selector">$1</span>{');
            } else if (str === 'x' && this.pxRegex.test(fullText.slice(-2))) {
                // 匹配样式单位
                fullText = fullText.replace(this.pxRegex2, '<span class="value px">px</span>');
            } else {
                fullText += str
            }
            // console.log(fullText)
            return fullText
        },
        // 检查cookies 用来判断用户是否第一次进入
        checkCookies() {
            if (Cookies.get("isCome")) {
                this.isCome = parseInt(Cookies.get("isCome"))
                console.log(this.isCome)
            } else {
                this.isCome = 0
                Cookies.set("isCome", 1, { expires: 7 })
                console.log(this.isCome)
            }
        },
        jumpToCV(){

        },
        shareIcon(){

        }
    }
}
</script>
<style lang='scss' scoped>
  .blogBox {
    position: relative;
    .flexBox{

      font-size: 40px;
      cursor: pointer;
      position:absolute;
      z-index: 10000;
      right: 10%;
      bottom: 5%;
    }
  }
</style>