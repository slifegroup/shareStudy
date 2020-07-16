## npm 全局安装，找不到对应的命令

 全局安装 `docsify`  

```
npm i docsify-cli -g
```

安装后，输入`docsify` 命令，显示无法找到对应命令

```
C:\Users\xulia>docsify
'docsify' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
```

win 10 下出现这个问题，大概问题是没有添加对应的目录到系统path

`npm` 全局安装，默认将包安装在 `C:\Users\xulia\AppData\Roaming\npm\node_modules` 目录下。知道包的目录，我们将 `C:\Users\xulia\AppData\Roaming\npm` 添加到系统环境中。再次输入命令

```
C:\Users\xulia>docsify
Usage: docsify <init|serve> <path>

Commands:
  docsify init [path]   Creates new docs
  docsify serve [path]  Run local server to preview site.
  docsify start <path>  Server for SSR

Global Options
  --help, -h     Show help                                             [boolean]
  --version, -v  Show version number                                   [boolean]

Documentation:
  https://docsifyjs.github.io/docsify
  https://docsifyjs.github.io/docsify-cli

Development:
  https://github.com/docsifyjs/docsify-cli/blob/master/CONTRIBUTING.md


[ERROR] 0 arguments passed. Please specify a command
```

成功！！

由于全局安装npm，默认是存放C盘。现在我们修改全局包的存放位置

```
npm config set prefix "E:\Program Files\nodejs\node_global" ## 设置全局安装的模块目录
npm config set cache "E:\Program Files\nodejs\node_cache" ## dui'y
```



