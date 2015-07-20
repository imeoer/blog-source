title: 一个写作与阅读平台的设计，初版纸小墨
date: 2015-07-19 10:00:00
author: me
draft: false
topic: -/images/inkpaper/bg.jpg
tags:
    - 产品
    - 设计
    - 写作
preview: 2014年上半年，我和同班毕业的小伙伴发起了一个业余小组的内容发布平台项目。创建一个类似Medium、简书的写作与阅读平台，专注改善文章撰写、阅读的交互体验，优化知识类文章发布与分享方式

---

> 题图来自Pixiv站画师TID作品《Sunny Rain》。

2014年上半年，我和同班毕业的小伙伴发起了一个业余小组的内容发布平台项目。创建一个类似[Medium](https://medium.com/)、[简书](http://jianshu.io/)的写作与阅读平台，专注改善文章撰写、阅读的交互体验，优化知识类文章发布与分享方式。

前端后端,与两大移动端开发各一位同学负责，主要技术栈有Backbone，Node.js，Python，MySQL，Golang，MongoDB与iOS，Android。我们有每周末的例会，探讨问题同步进度，用Google Doc记录想法，Trello管理任务。

前期设计到前后端基础实现断断续续持续了2个月，中期Web端完成了基本功能，但后期因为项目小组的业余开发时间不充裕，能在同一时间远程探讨问题和联调的时间很少，与上线运营做内容托管的难度，最终放弃，长期搁置该了项目。碰巧自己有记录一些东西的打算，我转而把这个想法写成了现在的[静态博客构建工具](http://www.inkpaper.io/blog/post/2015/03/01/ink-blog-tool.html)，搭建了这个个人博客。

Web端设计保持简洁，简化了与内容无关的界面元素，以内容组成界面，主题色为黑白，切合阅读风格。

![Web端登录与注册界面](-/images/inkpaper/login.png)

尽可能减少视觉干扰，使用顶部Loading Bar与弹出Tooltip提示。

![登录注册通用提示](-/images/inkpaper/tip.png)

纯文字元素与留白。

![首页视图](-/images/inkpaper/index.png)

![圈子列表设置视图](-/images/inkpaper/circle.png)

文章撰写分可视化与Markdown视图，可在右侧即时切换，可视化视图中使用类Medium的行内弹出式工具栏形式。

![文章撰写视图，初始化](-/images/inkpaper/edit_blank.png)

![文章撰写视图，类Medium格式工具栏](-/images/inkpaper/edit_toolbar.png)

![文章撰写视图，添加图片，音乐与视频工具栏](-/images/inkpaper/edit_add.png)

![个人文章视图](-/images/inkpaper/person.png)

![设置视图](-/images/inkpaper/setting.png)

![设置视图，多条目输入](-/images/inkpaper/setting_more.png)

![移动端启动屏，最初的拟定名片时文书](-/images/inkpaper/mobile.jpg)

Web前端与后端源码: [https://github.com/imeoer/bamboo](https://github.com/imeoer/bamboo)

项目失败了，收获很多，应该舍弃一些不成熟的想法，设想与解决问题更多的考虑全局。