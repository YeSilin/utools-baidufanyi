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
/* 清除全部最小宽度限制，但排除右侧机器人小图标 */
*:not(.qphmPPyw *) {
  min-width: 0 !important;
}

/* 屏蔽底部广告 */
.Hu5qsRSB {
  display: none !important;
}

/* 屏蔽输入框内多余文档翻译入口 */
.rfhEM3lg.bxgG6w8l {
  display: none !important;
}

/* 屏蔽多余的左边距 */
.L0kERzJV {
  padding: 0 0 0 0 !important;
}

/* 屏蔽多余的下边距 */
.Hu5qsRSB+div {
  display: none !important;
}
.qphmPPyw {
  height: 100% !important;
}

/* 屏蔽多余的右边距 */
.qphmPPyw {
  right: 0 !important;
  margin-left: 1px !important;
}

/* 屏蔽导航栏广告 */
.MMqloUXF>div:nth-child(1),
.MMqloUXF>div:nth-child(5),
.ZqJhu4sT,
.UzOvH9bK,
.operation-pos-new {
  display: none !important;
}

/* 导航栏布局优化 */
.lTKZuXrx:after {
  margin-right: 0 !important;
}

/* 屏蔽导航栏 */
.GXuSnnox {
  display: none !important;
}

/* 只留下文本翻译，不了都删掉 */
.yA48tp_a.huWexV6u {
  display: none !important;
}
.Ku91ofAV,
.b15UFYYv {
  top: 0 !important;
}
#editor-text {
  height: 100% !important;
}

/* 改一下间距 */
.ZhDN2yBJ {
  padding: 0 1px 0 0px !important;
}

/* 去掉右下角客服和收藏夹 */
.S1oAKzr9,
.AvGcOE_K.ARnMfncW {
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

  // document.querySelector("div.yoRr80Di > button > span").innerHTML="解锁字数上限"
}


// 获取闲置的 ubrowser
const idleUBrowsers = utools.getIdleUBrowsers();
// const ubrowser = utools.ubrowser.viewport(1024, 600).hide().goto(url).css(css).evaluate(evaluateCallback).show();
const ubrowser = utools.ubrowser.viewport(900, 500).goto(url).css(css).evaluate(evaluateCallback)

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
