# 熟悉mininet
https://mininet.org/（了解mininet如何运行测试）


## 安装VM虚拟机是最快的方式

## 虚拟机配置
默认的虚拟机只有8G的磁盘空间，因此需要进行扩容，参考这篇文章https://blog.csdn.net/qq_34160841/article/details/113058756。

需要注意的点如下

1. 没有图形化的桌面，无法使用gparted，可以安装轻量级的Xfce桌面环境
    ```bash
    sudo apt-get install xfce4 xfce4-goodies
    # 安装显示管理器
    sudo apt-get install lightdm
    #重启
    sudo reboot
    ```

## vscode远程连接虚拟机
用vscode开发比较方便，不用安装vmtools


## 需要在实验时注意的点
1. 当发生崩溃或者其他意外情况时，可以用这个命令清理网络环境
    ```bash
    sudo mn -c
    ```
2. iperf测试可以用来测带宽
3. Mininet won't write your OpenFlow controller for you; if you need custom routing or switching behavior, you will need to find or develop a controller with the features you require.
4. 可以模拟真实时间，但是如果硬件只能支持10Gbps的网络速度，那么在Mininet里模拟100Gbps的网络是不现实的。
5. h1到h2第一次发包时间会比较长，因为第一次下发flow entry之后就可以节省时间了。
6. 使用--mac参数主机可以获取到一个小的mac地址，而不是随机的，但是交换机的仍然会保持随机
7. 可以选择交换机类型，默认的是内核空间的交换机。还有用户空间的交换机，延迟会高得多，除此之外还有一种ovsk的交换机类型，是一种高度优化的，正常应该和内核相似甚至更快。
8. 不做任何测试就可以得到建立和拆除拓扑的时间,这样减去这部分可以获得更精确的时间。
    ```bash
    sudo mn --test none
    ```
9. ssh连接示例
```bash
sudo ~/mininet/examples/sshd.py
```
10. /hwintf.py里写了有关真实硬件接口接入mininet网络的方法
## 尚未解决的问题
1. Could not load the Qt platform plugin "xcb" in "" even though it was found.在安装了图形化桌面环境的虚拟机上运行不会报这个错，但是需要在本地显示的话，应该需要安装X11客户端来显示远程计算机上的图形化应用程序。FAQ里也有这个问题，但是不太一样
2. 可以使用 --innamespace 选项可以将每个主机、交换机和控制器放在各自独立的网络命名空间中，目前不清楚有什么用。
3. 不同类型的交换机，在具体使用场景上的不同。
## Walkthrough中可能疑惑的点
1. Part4中ssh daemon部分，ssh连接主机之后好像也没啥变化，而且无论ssh连不连接host1，都能够ping host2，其实是因为不ssh连接时，交换机在主进程的命名空间中也能ping通host2。可以通过ifconfig看出来ssh连接host1和不连接的区别。


## In-network Aggregation
https://github.com/ZER0-Nu1L/In-network-Aggregation


# 远程抓包

## 远程机器配置

确保普通用户能抓包
```bash
sudo setcap cap_net_raw,cap_net_admin=eip $(which tcpdump)  #允许非root用户运行tcpdump
tcpdump -i eth0(根据ifconfig选择)
```


# 服务器部署mininet
```bash
# 执行到安装部分时，原本提供的install.h有很多错误，将mininet/util/install.h更换为当前仓库目录下的install.h并运行：
mininet/util/install.h -a
# 将mn可执行文件复制到/usr/local/bin/下才能直接使用
sudo cp /home/dd/mininet/bin/mn /usr/local/bin/
```

