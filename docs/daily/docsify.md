## doscify 使用说明

1. 全局安装 `docsify-cli`

   ```
   npm i docsify-cli -g
   ```

   注意：如果安装成功后，出现输入`docsify` 命令，显示无法找到对应命令，具体参考 [npm 全局安装，找不到对应的命令](daily/npm全局安装找不到命令.md)

2. 初始化项目

   ```
   docsify init ./docs
   ```

3. 使用 `docsify serve` 运行项目，默认通过`http://localhost:3000` 地址访问网站

   ```
   docsify serve docs
   ```

   











### 推送 `github` 注意

文档需要建在 `docs` 目录下，你也可以创建子目录，写好文件后记得在 `_sidebar.md` 文件里配置菜单，具体写法如下：

> 使用无序列表创建树型目录，文档指向使用 `[文件名](文件地址)`

例子：

* 问题点
  * `[测试](daily/doscify.md)`



