---
title: hexo笔记
date: 2019-10-28 10:13:34
tags: hexo
password: 888888
---
[hexo的next主题个性化教程:打造炫酷网站](http://shenzekun.cn/hexo%E7%9A%84next%E4%B8%BB%E9%A2%98%E4%B8%AA%E6%80%A7%E5%8C%96%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B.html)
***
[next官方配置教程](http://theme-next.iissnan.com/theme-settings.html#reward)
***
[超详细Hexo+Github Page搭建技术博客教程](https://juejin.im/post/5c4730c9f265da61616efeec#heading-25)
***
[next主题优化定制修改指南***](https://blog.csdn.net/u012195214/article/details/79204088)
***
[hexo主题库](https://hexo.io/themes/)
***
[博客示例](https://gongchenghuigch.github.io/)
***
[next官方文档教程中文](http://theme-next.iissnan.com/faqs.html)
***
[添加网易云音乐](https://www.jianshu.com/p/d747148cffad)
***
### 增加文章搜索功能
安装插件`hexo-generator-searchdb`,执行以下命令:
`npm install hexo-generator-searchdb --save`
修改`hexo/_config.yml`站点配置文件,末尾新增以下代码:
```
search:
  path: ./public/search.xml
  field: post
  format: html
  limit: 10000
```
修改`themes/next/_config.yml`主题配置文件，搜索关键字`local_search`找到如下代码，将`enable`设置为`true`，如下：
```
local_search:
  enable: true
```