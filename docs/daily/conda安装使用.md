### conda  安装和使用

conda 主要好处是独立一个虚拟环境，每个环境的依赖模块互不影响，每个环境都独立一个python环境。如果创建的虚拟环境不想使用，直接删除即可，灰常方便

优点：使用conda程序包管理器的优势在于，它为所有平台（Windows，Mac，Linux）的GeoPandas的所有必需和可选依赖项提供了预构建的二进制文件。

1. 安装 conda 

   官方文档 https://docs.anaconda.com/anaconda/install/windows/

   **安装过程中建议不要选择添加`Path`环境**

2. 使用 conda

   官方文档 https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html

   1. 打开 Anaconda Prompt.

   2. 创建一个虚拟环境，

      ```
      conda create --name xula-test
      ```

   3. 激活环境

      ```
      activate xula-test
      ```

   4. 在环境中安装需要的包

      ```
      conda install beautifulsoup4 ## 安装beautifulsoup4包
      conda list ## 查看安装的包列表
      ```

      主要如果安装出现找不到对应的包的情况，访问这个网站 https://anaconda.org/search?q=geopy，输出你的包

   5. ​	查看 conda 创建环境的目录

      ```
          (base) C:\Users\xulia>conda env list
          # conda environments:
          #
                                   D:\Users\xulia\miniconda3\package
          base                  *  d:\Users\xulia\miniconda3
          xula-test                d:\Users\xulia\miniconda3\envs\xula-test ## 环境对应的目录
      ```

3. 在 pycharm 中使用 conda 环境

   使用  `xula-test` 环境，目录 `d:\Users\xulia\miniconda3\envs\xula-test`

   ![image-20200620151711196](https://img.mupaie.com/image-20200620151711196.png)

问题解决：

1. canda 安装的 numpy 使用不了，numpy 需要使用 pip 安装

   ```shell
   pip install --upgrade --force-reinstall numpy 
   ```

2. 关于使用 geopy 报以下错误，安装最新的版本 `openssl`，下载地址 https://slproweb.com/products/Win32OpenSSL.html

   ```shell
   cannot import name 'HTTPSHandler' from 'urllib.request' (D:\Users\xulia\miniconda3\lib\urllib\request.py)
   ```

   解决方法来源于：https://github.com/ContinuumIO/anaconda-issues/issues/10592

**注意：以后如果需要安装其他依赖模块，请在 `Anaconda Prompt` 命令框下，激活自己的环境 安装其他包**































