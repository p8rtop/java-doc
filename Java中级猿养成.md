# Java中级猿养成

作者：诺贝尔科技进步奖

ps：视频请两倍速观看，视频内容都不是提前准备的素材，我只会去准备大纲，然后再一步一步的去做。学习最重要的除了结果之外，还有学习解题过程。一句话，好记性不如搜索引擎，小问题百度，大问题谷歌。

# 1. 开发环境

## 1.1 创建Centos7.9虚拟机

### 1.1.1 VMware 17.0.0 安装

如何安装（一定要学会使用搜索引擎，小问题百度，大问题谷歌）

https://www.vmware.com/products/workstation-pro.html

秘钥百度一下，多试试就好了

没有包，不知道怎么下载的可以给我私信要

不建议用网上那些什么破解机的，容易搞病毒

### 1.1.2 CentOS-7-x86_64-Minimal-2009.iso 镜像下载

https://www.centos.org/download/

Older Versions-->click here

http://isoredirect.centos.org/centos/7/isos/x86_64/

官网多看看，实在不行就翻译下，找找就找到了，要学会自己上官网找东西

chrome有一些插件可以装一下，代理和翻译是我的必装插件

没有包的，可以私信我下，但是建议自己下载，这也是程序员的必要的修养

### 1.1.3 创建宿主机

选择第一项install，就开始安装了

语言选英语

时间选亚洲-上海

默认就是最小安装

选择一下硬盘

连一下网，时间就自动更新了

设置一下root密码，弱密码的话，会提示你，点两次Done确认就行了

接下来就是自动安装时间



Ctrl+Alt 快捷键，那个鼠标就会从宿主机里面跳出来

### 1.1.4 关闭防火墙

登录进去，看一下IP

命令是 ip address，我这IP是192.168.84.142

en33就是网卡，网上很多固定IP，我这里不建议，有虚拟机之后很方便，IP变了的话，进去自己命令看一下就行了

用连接工具比较舒服，我推荐finalShell 很便宜，三十多块钱终身版

永久关闭防火墙 systemctl disable firewalld.service

## 1.2 宿主机Centos7.9拓展硬盘和内存

### 1.2.1 VMware拓展内存

内存拓展特别简单，直接虚拟机配置改一下就行了

前提是宿主机先关机，开机和挂起状态下都不行

编辑虚拟机配置-->内存-->拖拽即可-->保存

然后就搞完了

### 1.2.2 Vmware Centos磁盘拓展容量

1-设置中拓展内存，我原本是20G，想增大10G，就改成30G

### 1.2.3 宿主机挂载磁盘

磁盘拓展麻烦一点，根本记不住，因为记住也没什么用，以后进入公司有运维搞，除非你是小公司，如果是小公司，那就百度一下，完事了

同样需要先关机

直接百度 Vmware Centos磁盘拓展容量

自己找找，就直接开搞 https://www.cnblogs.com/raisins/p/13224353.html

可以保存一下PDF（Ctrl+P 打印选择保存PDF即可），这样以后就不用去百度再找了，就作为了自己的知识库

按照文档，开搞

1-设置中拓展内存，我原本是20G，想增大10G，就改成30G

2-   fdisk -l  看到Linux LVM这种磁盘，那就是你拓展的那10G的磁盘

3-   fdisk /dev/sda

4-   n 创建新分区

5-   文章中让敲回车，是因为只有一个拓展虚拟的磁盘，如果你设置了多个，就输入对应的号就行，我这里选择 p 磁盘

6-     然后就Enter，一直出现   分区 3 已设置为 Linux 类型，大小设为 10 GiB

7-     w   写入分区表，保存

8-     fdisk -l  就会看到新出来的磁盘sda3

9-     partprobe /dev/sda3   (啥玩意，出错了)

特娘的，需要重启，这是我从前百度的文章（https://blog.csdn.net/Cecidit_824/article/details/131245349）

10-    重启机器 reboot

11- pvcreate /dev/sda3

12- pvcreate /dev/sda3   （对应的/dev/sda3需要看你的是多少，可以用fdisk -l来看）

13-     cd /dev/mapper/ | ls （查看自己的文件夹）

14-    df -h (现在可用是/dev/mapper/centos-root  16G，接下来就是给它拓展)

15-     vgextend /dev/mapper/centos /dev/sda3

16-    lvextend -L +9G /dev/mapper/centos-root （加的虽然是10G，但是实际没有10G，所以你写小点就完事了）

17-    xfs_growfs /dev/centos/root

18-    df -h （/dev/mapper/centos-root   26G  1.3G   25G    5% ）

这样就拓展完毕了。

不知道我这fdisk -l咋还显示那个Linux LVM（我记得拓展完了之后好像会消失的，算了，不纠结，不重要，开发环境，能用就完事了，都是测试环境，无所谓，不要纠结自己不熟悉的东西，这些东西是运维学的，不是后端学的，了解即可）

以后的虚拟机使用中，你就会碰到虚拟机硬盘一开始规划小了的问题，这时候就需要自己拓展一下，很简单的。但是不会的话，就只会像我一开始那样，不够了就删虚拟机，装完了，又重新搞docker挺无语的。

## 1.3 开发环境-4K高清壁纸

工欲善其事必先利其器，优雅的背景桌面会让人开发时快速进入佳境。

废话少说，上网址：https://wallhaven.cc/toplist?page=1

全是好壁纸，平时我比较喜欢没事就找些好看的壁纸，这些壁纸可以按照你的电脑尺寸就行下载，这样就是最完美的匹配。

查看电脑分辨率：设置-->系统-->屏幕-->显示器分辨率  （我这里是2880*1800）

点开让你心动的壁纸：Crop & Scale Download 输入你的显示器分辨率 下载即可

然后就可以愉快的敲代码了

## 1.4 开发环境-Centos7.9安装docker

国庆节过完上班好困啊。。。。

安装docker  https://m.runoob.com/docker/centos-docker-install.html

```
# 卸载旧的安装
sudo yum remove docker \
         docker-client \
         docker-client-latest \
         docker-common \
         docker-latest \
         docker-latest-logrotate \
         docker-logrotate \
         docker-engine

# 设置仓库
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
 
# 阿里云
sudo yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    
# 安装社区版Docker最新版
sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin

# 设置docker自启动
systemctl enable docker.service

# 启动docker
sudo systemctl start docker

# 测试镜像
sudo docker run hello-world
```

完事了，安装好了,再介绍一些常用命令

```
 # 查看运行中的容器
 docker ps
 # 查看所有容器，包含运行中和已结束的
 docker ps -a
 # 查看镜像
 docker images
 # 删除镜像
 docker rmi [IMAGE ID]
 # 删除容器
 docker rm [CONTEINER ID]
 # 启动容器
 docker start [CONTEINER ID]
 # 重启容器
 docker restart [CONTEINER ID]
 # 停止容器
 docker stop [CONTEINER ID]
```



一定要弄清楚Docker的两个概念。

镜像和容器

镜像就是一些封装的最小系统，容器就是基于镜像创建出来的可执行程序（docker run ...）

就像win的vmware虚拟机，镜像就是ISO文件，容器就是我们虚拟出来的宿主机

很好理解。我们想要屏蔽环境差异，从而快速创建好开发环境，就直接拉取别人的镜像，然后创建你的容器，就完事了

过两天我再录个快速搞redis和mysql的



今天就到这里吧，谢谢观看。









