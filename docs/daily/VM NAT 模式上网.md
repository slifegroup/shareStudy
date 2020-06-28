VM 下 配置NAT 模式上网

* **环境**

**VMware workstation 14.0**

**联网主机 Win10一台**

* **外部网络设置**

打开 控制面板 -> 网络和 Internet -> 网络和共享中心，单击 更改适配器设置

![](https://img.mupaie.com/20200617150541.png)

因为主机使用wifi上网，选择 WLAN 模式，右击 WLAN -> 选择共享 -> 勾选 允许其他网络用户通过此计算机的Internet连接来连接(N) -> 文本框下选择 VMware Network Adapter VMnet8  即VMware NAT连接模式的网卡

![](https://img.mupaie.com/20200617150647.png)

右击 VMware Network Adapter VMnet8 网络，右键点属性 -> 选择 internet协议版本4，设置子网IP地址及DNS

![](https://img.mupaie.com/20200617150959.png)

* **VMware 中设置虚拟机网络**

1. 打开 VMware ，点击编辑 -> 选择虚拟网络编辑器 -> 选择 VMnet8 并进行相应设置

![](https://img.mupaie.com/20200617151118.png)

点击 DHCP setting (DHCP服务器设置)，设置相应选项

2. 点击 NAT setting(NAT设置)，设置相应选项，这里 **网关 IP** 网段需要跟 VMnet8 中的 IP 网段 **相同 **

![](https://img.mupaie.com/20200617151350.png)

以上就是虚拟机NAT模式上网的步骤，下面我们通过连接虚拟机进行网络测试

通过 本机ip + 端口(10300) , 实现虚拟机的连接，测试网络

![](https://img.mupaie.com/20200617152514.png)

