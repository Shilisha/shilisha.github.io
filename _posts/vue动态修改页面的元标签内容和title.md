---
layout:     post
title:      vue单页面开发的动态修改meta元标签的关键词和描述，页面title名
subtitle:   利用路由守卫控制页面的跳转,方便搜索引擎的友好，利于SEO
date:       2019年4月24日
author:     Lisa
header-img: img/post-bg-debug.png
catalog: true
tags:
    - SEO
    - 搜索引擎友好
    - 路由守卫
    - 元标签aaa
---



## meta标签和title

vue这个框架，是个单页面开发，在很多时候，并不利于SEO，我这边刚好在开发一个公司的官网，需求是需要每个页面要有对应的meta标签的description&keywords以及title的名字

 
 
## 更改路由index文件
 ###创建一个路由对象
const router = new Router({
  routes: [
    // 首页
    {
      path: '/',
      name: 'index',
      meta: {
        title: '这个是首页',
        keywords: '这个是首页',
        description: '这个是首页'
      },
      component: Index
    },
    // 产品页
    {
      path: '/pro',
      name: 'pro',
      meta: {
        title: '产品页',
        keywords: '产品页',
        description: '产品页'
     }
  ]
})
###在这个对象的基础上添加路由守卫，记得在根文件index.html里面添加空的元标签哦
1.index.html:
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <meta name="keywords" content="XXXXXXXXXXX">
    <meta name="description" content="XXXXXXXXXX">
    <title>XXXXXXXXXX</title>
  </head>
 2.router/index.js
router.beforeEach((to, from, next) => {
  /* 路由发生变化修改元标签的关键词和描述，用来SEO */
  var metaList = document.getElementsByTagName("meta");
  if (to.meta.title) {
    /* 路由发生变化修改页面title */
    //循环遍历每一个元标签
    document.title = to.meta.title
    for (var i = 0; i < metaList.length; i++) {
      if (metaList[i].getAttribute("name") == "keywords") {
        metaList[i].content = to.meta.keywords;
      }else if (metaList[i].getAttribute("name") == "description"){
        metaList[i].content = to.meta.description;
      }
    }
  }
  next()
})
####记得一定要执行next()这个方法哦
