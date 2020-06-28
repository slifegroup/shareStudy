## 对 VMware Workstation 虚拟机下的  centos7 扩容

1、 首先在虚拟机中对系统磁盘进行扩容，下面以VMware中安装的centos7为例：

<img src="https://img.mupaie.com/20200616095055.png" style="zoom: 67%;" />

点击上图中的扩展按钮，并将原本的硬盘空间从 30GB扩展到 40GB。

扩容前我们先了解一下逻辑卷管理

> ​	逻辑卷管理LVM是一个多才多艺的硬盘系统工具。无论在Linux或者其他类似的系统，都是非常的好用。传统分区使用固定大小分区，重新调整大小十分麻烦。但是，LVM可以创建和管理“逻辑”卷，而不是直接使用物理硬盘。可以让管理员弹性的管理逻辑卷的扩大缩小，操作简单，而不损坏已存储的数据。可以随意将新的硬盘添加到LVM，以直接扩展已经存在的逻辑卷。LVM并不需要重启就可以让内核知道分区的存在。

LVM使用分层结构，如下图所示。

![img](https://pic2.zhimg.com/80/v2-3a750a7a7b9b72834d1b51839a079acd_720w.jpg)

图中顶部，首先是实际的物理磁盘及其划分的分区和其上的物理卷（PV）。一个或多个物理卷可以用来创建卷组（VG）。然后基于卷组可以创建逻辑卷（LV）。只要在卷组中有可用空间，就可以随心所欲的创建逻辑卷。文件系统就是在逻辑卷上创建的，然后可以在操作系统挂载和访问。

1. 查看磁盘文件可用空间，发现可用磁盘空间只有20G

```shell
[root@localhost ~]# df -h
```

![](https://img.mupaie.com/20200616095540.png)

2. 查看磁盘空间的详细信息，有一个32.2G的磁盘

```shell
[root@localhost ~]# fdisk -l
```

![](https://img.mupaie.com/20200616095737.png)

​	对一些参数解释

* Device：分区的设备文件名称  Boot:是否是引导分区，即开机区，若是，则用“*”标识
* Start ：开始柱面，即该分区在硬盘中的起始位置   End ：结束柱面，即该分区在硬盘中的结束位置
* Blocks：分区的大小，以Blocks（块）为单位，默认的块大小为1024字节，即1KB
* Id：分区类型的ID标记号，对于`EXT3`分区为`83`，LVM 分区为 `8e`   
* System：分区类型，即磁盘分区内的系统

3. 创建磁盘分区，使用‘8e’类型来使其可用于LVM，**LVM 分区为 `8e`**   

```shell
[root@localhost ~]# fdisk /dev/sdb 
```

![](https://img.mupaie.com/20200616101615.png)

创建分区后，重启系统，目的：**使`kernel`使用新的表**

4. 创建物理卷

```shell
[root@localhost ~]# pvcreate /dev/sda4
Physical volume "/dev/sda4" successfully created
```

5. 检查物理卷的创建情况

```shell
[root@localhost ~]# pvdisplay
```

![](https://img.mupaie.com/20200616102603.png)

6. 扩展卷组

```shell
[root@localhost ~]# vgextend centos /dev/sda4
  Volume group "centos" successfully extended
```

7. 查看卷组

```shell
[root@localhost ~]# vgdisplay 
```

![](https://img.mupaie.com/20200616102822.png)

8. 查看逻辑卷

   ```shell
   [root@localhost ~]# lvdisplay
   ```

![](https://img.mupaie.com/20200616103227.png)

9. 给对应的逻辑卷扩容  

```shell
[root@localhost ~]# lvextend -l +100%FREE /dev/centos/root 
  Size of logical volume centos/root changed from 26.76 GiB (6851 extents) to <31.99 GiB (8189 extents).
  Logical volume centos/root successfully resized.
```

10. 对`LV`执行容量调整

```text
[root@localhost ~]# xfs_growfs /dev/centos/root 
```

![](https://img.mupaie.com/20200616103749.png)

11. 查看磁盘空间，可以看到扩容成功

    ```shell
    [root@localhost ~]# df -lhT
    ```

![image-20200616103904566](C:\Users\xulia\AppData\Roaming\Typora\typora-user-images\image-20200616103904566.png)

​		注意：**虽然xfs文件系统只支持增加，不支持减少。但并不是说在xfs系统文件下不能减小，只是减小后，需要重新格式化才能挂载上。这样原来的数据就丢失了！设置大小的时候需谨慎**

以上操作完成了对centos的扩容，下面讲几个文件系统

*  /dev/mapper/centos-root 的挂载点是 / (根目录)，即通常所说的根分区或根文件系统

* /dev/sda1 (boot分区) 中保存了内核映像和一些启动时需要的辅助文件

* /dev/mapper/centos-home 用户家目录单独做了分区 注意：**这个分区和根分区是独立的**

更详细的 lvm 操作请看 ☞ [LVM 中文版 ](https://wiki.archlinux.org/index.php/LVM_(简体中文)) 或 [LVM 英文版](https://wiki.archlinux.org/index.php/LVM)