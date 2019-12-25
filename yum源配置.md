### yum源配置
1. 直到yum源的老家在哪？
```
cd /etc/yum.repos.d/
进入到该文件夹时可以看一下有哪些yum源文件，所有以*.repo结尾的就是yum源文件
```
2. 可以将原有的yum放在一个文件夹里备份。
```
[root@localhost yum.repos.d]# mv *.repo ./yum_repo
```
3. 将linux的yum源改为aliyun的yum源仓库
    1. 找到阿里巴巴的yum网站
    ```
    https://developer.aliyun.com/mirror/centos?spm=a2c6h.13651102.0.0.53322f70muf4b1
    ```
    2. 找到centos标签，找到centos7的相关命令如下：
    ```
    wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
    ```
    3. 在linux上运行以上命令
    4. 清空原本的yum缓存
    ```
    yum clean all
    ```
    5. 安装linux的额外仓库，也就是epel源，继续在阿里云源上下载。
    ```
	wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
    ```
    6. 生成yum的缓存，以便以后加速下载
    ```
    yum makecache
    ```
### yum常用参数
```
yum repolist all        列出所有仓库
yum list all            列出仓库所有软件包
yum info 软件包名            查看软件包信息
yum install 软件包名        安装软件包
yum reinstall 软件包名    重新安装软件包
yum update    软件包名        升级软件包
yum remove    软件包名        移除软件包
yum clean all            清楚所有仓库缓存
yum check-update        检查可以更新的软件包
yum grouplist            查看系统中已安装的软件包
yum groupinstall 软件包组    安装软件包组
```

### 系统服务的相关命令
```
systemctl start redis       启动
systemctl restart redis     重启 
systemctl stop redis        停止
systemctl status redis      看状态
```
### 常用的linux目录
```
#网卡配置文件
/etc/sysconfig/network-script/ifcfg-eth0
#修改机器名以及网卡，网管等配置
/etc/sysconfig/network
#linux的dns客户端配置文件，实现域名和ip的互相解析
/etc/resolv.conf
#本地dns解析文件,设定ip和域名的对应解析,开发测试最常用的临时域名解析
/etc/hosts/
#系统全局环境变量永久生效的配置文件,如PATH等
/etc/profile
#用户的环境变量
~/.bash_profile 
~/.bashrc
#存放可执行程序的目录，大多是系统管理命令
/usr/sbin
#存放用户自编译安装软件的目录  > 等同于C:\Program files （windows）
/usr/local
#关于处理器的信息,还可以top指令查看
/proc/cpuinfo
#查看内存信息，还可以free -m
/proc/meminfo
```