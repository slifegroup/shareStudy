1. 拉取远程代码报错：

   ```shell
   Error merging: refusing to merge unrelated histories
   ```

   * 原因：由于该分支没有相关联的历史信息

   * 解决：git pull --allow-unrelated-histories

     --allow-unrelated-histories： 允许未关联的历史文件合并

   

