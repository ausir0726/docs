---
title: 如何集成 Google 统计分析服务？
description: 如何集成 Google 统计分析服务？
---

# 如何集成 Google 统计分析服务？

在 Nuxt.js 应用中使用 [Google 统计分析服务](https://analytics.google.com/analytics/web/) ，推荐在 `plugins` 目录下创建 `plugins/ga.js` 文件：

```js
/* eslint-disable */
import router from '~router'
/*
** 只在生成模式的客户端中使用
*/
if (process.env.NODE_ENV === 'production') {
  /*
  ** Google 统计分析脚本
  */
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');
  /*
  ** 当前页的访问统计
  */
  ga('create', 'UA-XXXXXXXX-X', 'auto')
  /*
  ** 路由行为改变时每次触发(初始化同样触发)
  */
  router.afterEach((to, from) => {
    /*
    ** 告诉 Google 统计分析服务 增加新的页面访问统计
    */
    ga('set', 'page', to.fullPath)
    ga('send', 'pageview')
  })
}
```

> 记得将 `UA-XXXXXXXX-X` 替换成你的 Google 统计分析服务的跟踪编号。

然后，我们需要告诉 Nuxt.js 将该插件导入主应用中：

`nuxt.config.js`
```js
module.exports = {
  plugins: [
    { src: '~plugins/ga.js', ssr: false }
  ]
}
```

恭喜，你的 Nuxt.js 应用成功集成了 Google 的统计分析服务。

<p class="Alert Alert--nuxt-green"><b>提示：</b> 你可以用相同的方法集成别的统计分析服务。</p>
