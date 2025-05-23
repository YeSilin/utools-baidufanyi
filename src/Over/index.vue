<script lang="ts" setup>

// 获取全部传参
const props = defineProps({
  enterAction: {
    type: Object,
    required: true,
  },
});


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



// 百度翻译链接，加个时间戳以便复用浏览器的时候强制页面刷新
// const url = `https://fanyi.baidu.com/mtpe-individual/multimodal?query=${payload}&lang=${lang}&t=${Date.now()}`
const url = `https://fanyi.baidu.com/mtpe-individual/multimodal?query=${payload}&lang=ait2zh&t=${Date.now()}`


// /* 屏蔽通用领域学术论文一整行 */
// .qJU3axmS,.dfAvRK2O {
//   display: none !important;
// }
// 注入css去广告
const css = `
/* 屏蔽点击或拖拽上传 */
.ant-upload.ant-upload-drag .ant-upload-drag-container {
  display: none !important;
}

/* 屏蔽花里胡哨的彩色图片秒翻字体 */
.in7aMwxd {
    background: none !important; /* 取消背景渐变 */
    -webkit-text-fill-color: inherit !important; /* 恢复正常文字颜色 */
    -webkit-background-clip: border-box !important; /* 取消文本裁剪 */
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

/* 屏蔽点赞按钮 */
.fGdWpPpO.bLSwAXlU {
  display: none !important;
}

/* 屏蔽问问A助手 */
.XS9zPMly {
  display: none !important;
}

/* 屏蔽三横按钮 */
.lfsnR62F  {
  display: none !important;
}

/* 屏蔽买1送6图片广告 */
._m6jE1Mj {
  display: none !important;
}
/* 屏蔽多余的分割线 */
.PwoDe1T6 {
  padding: 10px 0 !important;
}
/* 屏蔽多余的分割线 */
.PwoDe1T6:before {
    border-top: none !important;
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


// 执行 JavaScript 注入的回调函数
function evaluateCallback() {
  // 点击元素收起导航栏
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
// const ubrowser = utools.ubrowser.viewport(1024, 600).hide().goto(url).css(css).evaluate(evaluateCallback).show();
const ubrowser = utools.ubrowser.viewport(1024, 600).goto(url).css(css).evaluate(evaluateCallback)

// 如果有闲置的 ubrowser，则使用它，否则创建新窗口
ubrowser.run(idleUBrowsers.length > 0 ? idleUBrowsers[0].id : null);




// utools.ubrowser.devTools()


// 直接退出插件，如果需要调试啥的，就注释掉
utools.outPlugin();



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
