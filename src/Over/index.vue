<script lang="ts" setup>

// 获取全部传参
const props = defineProps({
  enterAction: {
    type: Object,
    required: true,
  },
});


// 分析语言占比
function analyzeLanguage(text) {
  let chineseCount = 0;
  let englishCount = 0;

  for (let char of text) {
    let code = char.charCodeAt(0);
    if (code >= 0x4E00 && code <= 0x9FFF) chineseCount++; // 统计中文字符
    if ((code >= 65 && code <= 90) || (code >= 97 && code <= 122)) englishCount++; // 统计英文字符
  }

  let total = chineseCount + englishCount;
  if (total === 0) return "无法识别"; // 防止文本为空的情况

  let chineseRatio = chineseCount / total;

  return chineseRatio > 0.5 ? "中文占多数" : "英文占多数";
}


// 插件启动时立即隐藏窗口
utools.hideMainWindow();


// 获取传参，不修改原始传参
let payload = props.enterAction.payload


// 排除翻译和百度翻译这个关键字
if (payload === '翻译' || payload === '百度翻译') {
  payload = ''
} else {
  payload = encodeURIComponent(payload)
}

// 粗糙赋值
// let lang = analyzeLanguage(props.enterAction.payload) === '中文占多数' ? 'zh2en' : 'en2zh';


// 百度翻译链接，加个时间戳以便复用浏览器的时候强制页面刷新
// const url = `https://fanyi.baidu.com/mtpe-individual/multimodal?query=${payload}&lang=${lang}&t=${Date.now()}`
const url = `https://fanyi.baidu.com/mtpe-individual/multimodal?query=${payload}&lang=ait2zh&t=${Date.now()}`

// 注入css去广告
const css = `
/* 屏蔽点击或拖拽上传 */
.ant-upload.ant-upload-drag .ant-upload-drag-container {
  display: none !important;
}

/* 屏蔽学术论文按钮 */
.NttHa0mN>span:nth-child(2) {
  display: none !important;
}

/* 屏蔽实时翻译按钮 */
button.tCjFePWN {
  display: none !important;
}

/* 屏蔽AI大模型翻译按钮 */
.sRKHr8FI {
  display: none !important;
}

/* 屏蔽人工翻译按钮 */
.YGx8668_+.YGx8668_ {
  display: none !important;
}

/* 屏蔽AI论文精翻按钮 */
.h5JHRvHF {
  display: none !important;
}

/* 屏蔽专业译后编辑按钮 */
.jygLSCYa {
  display: none !important;
}

/* 屏蔽买1送6图片广告 */
._m6jE1Mj,._m6jE1Mj+.PwoDe1T6 {
  display: none !important;
}

/* 屏蔽编辑译文，添加术语，智能增强按钮 */
.cfm52tbf,.uPRUHmis,.QrzU1B4W {
  display: none !important;
}

/* 屏蔽进入词典模式 */
.o7b5Ojxh {
  display: none !important;
}

/* 屏蔽页尾 */
.sF3Yx_p0 {
  display: none !important;
}
`

// 点击元素收起导航栏
function clickElement() {
  const button = document.querySelector("#multiContainer > div.ZHrlRAUU > div > div.lslKUKjX > div.qJU3axmS > div.LxF9kyWA > span") as HTMLElement;
  if (button) {
    button.click(); // 自动点击按钮
    console.log("按钮已点击！");
  } else {
    console.log("未找到目标按钮");
  }
}


// 获取闲置的 ubrowser
const idleUBrowsers = utools.getIdleUBrowsers();
// console.log(idleUBrowsers);
if (idleUBrowsers.length > 0) {
  utools.ubrowser.goto(url).css(css).evaluate(clickElement).run(idleUBrowsers[0].id)
} else {
  utools.ubrowser.viewport(1024, 600).goto(url).css(css).evaluate(clickElement).run({});
}

// utools.ubrowser.devTools()


// 直接退出插件，如果需要调试啥的，就注释掉
utools.outPlugin();










// utools.ubrowser.goto('https://fanyi.baidu.com/mtpe-individual/multimodal?query=good').run()
// utools.ubrowser.goto(`https://fanyi.baidu.com/mtpe-individual/multimodal?query=${props.enterAction.payload}`).viewport(1024,600).run()
// utools.hideMainWindow();
// ubrowser.show()
// console.log(props.enterAction.payload);

</script>

<template>
  <div class="over">
    <h2>插件应用进入参数</h2>
    <pre>
{{ JSON.stringify(enterAction, undefined, 2) }}
{{ url }}
    </pre>
  </div>
</template>

<style>
.over {
  padding: 10px 28px;
}
</style>
