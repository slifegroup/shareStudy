## Hexo 构建个人博客，使用Travis CI 实现自动化部署，更新

### 开始工作

关于通过<kbd>Hexo</kbd>构建个人博客，官网的操作已经很详细了 [[官网地址](https://hexo.io/zh-cn/docs/)]。今天主要讲一次本人构建过程中遇到的几个问题

1. 使用第三方的主题 <kbd>NexT</kbd> 一些参数配置

   * 如何创建404页面

     ```
     hexo new page 404
     ```

     创建404页面修改后缀为.html,加上以下源码：

     ```
     <!DOCTYPE HTML>
     <html>
     <head>
       <meta http-equiv="content-type" content="text/html;charset=utf-8;"/>
       <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
       <meta name="robots" content="all" />
       <meta name="robots" content="index,follow"/>
     </head>
     <body>
     <script type="text/javascript" src="http://www.qq.com/404/search_children.js" charset="utf-8" homePageUrl="your site url " homePageName="回到我的主页"></script>
     </body>
     </html>
     ```

     使用GitHub的域名访问会出现空白页，问题待解决

     * 如何设置「阅读全文」？

     ​    在首页显示一篇文章的部分内容，并提供一个链接跳转到全文页面是一个常见的需求。` NexT` 提供三种方式来控制文章在首页的显示方式。 也就是说，在首页显示文章的摘录并显示 **阅读全文** 按钮，可以通过以下方法：

     1. 在文章中使用 `<!-- more -->` 手动进行截断，`Hexo` 提供的方式 **推荐**

     2. 在文章的 [front-matter](https://hexo.io/docs/front-matter.html) 中添加 `description`，并提供文章摘录

     3. 自动形成摘要，在 **主题配置文件** 中添加：

        ```
        auto_excerpt:
          enable: true
          length: 150
        ```

        默认截取的长度为 `150` 字符，可以根据需要自行设定，但是我配置了无效

     > 建议使用 ``（即第一种方式），除了可以精确控制需要显示的摘录内容以外， 这种方式也可以让 Hexo 中的插件更好的识别。

2. ` Travis CI` 实现自动化部署 ，更新的时候使用 `GH_TOKEN ` 上传到 `Github` 一直报无效的用户名，密码

   

