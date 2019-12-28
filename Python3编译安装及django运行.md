### 编译安装三部曲
1. 首先解决软件包依赖问题
```
yum install gcc patch libffi-devel python-devel  zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel -y
```
2. 下载软件的源代码(如python3.7.tgz)
3. 解压缩软件源代码，切换进入源代码目录
```
tar -xf Python-3.8.0.tgz
```
4. 进入python源代码目录，释放编译文件
```
./configure --prefix=/opt/python380/
```
5. 开始编译，编译安装
```
make -> make install
这两句可以合并为一句
make && make install
```
6. 检查自己制定的安装路径，python3的可执行命令都在bin底下
```
/opt/python380
/opt/python380/bin
```
7. 配置软连接 快捷启动python3和pip3
```
ln -s /opt/python380/bin/python3   /usr/bin/python
ln -s /opt/python380/bin/python3   /usr/bin/pip3
```
8. 将python的整个bin文件加入环境变量
```
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin:/opt/python380/bin
PATH=$PATH:/opt/python380/bin/
写入个人配置文件
vim /etc/profile 编辑该文件写入PATH
PATH=$PATH:/opt/python380/bin/
source /etc/profile 读取配置文件 生效配置
```
**至此，python3安装完毕**

### Django安装运行
1. 安装Django，这里安装的版本是2.1.4
```
pip3 install django==2.1.4
```
2. 创建Django项目
```
django-admin startproject mysite
```
3. 运行Django项目
```
python3 manage.py runserver ip地址:端口号
```
> 注意要切换到项目文件夹下运行manage文件，允许ip地址要在settings中添加，这里的ip地址设置为0.0.0.0，意味着虚拟机的任意一个网卡的ip地址都可以访问