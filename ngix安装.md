### nginx安装配置
1. 检查系统之前是否装过nginx，如果安装过就将其卸载
```
yum remove nginx -y
```
2. 解决软件包的依赖问题
```
yum install gcc patch libffi-devel python-devel  zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel openssl openssl-devel -y
```
3. 下载nginx源代码
```
wget -c https://nginx.org/download/nginx-1.12.0.tar.gz
```
4. 解压缩源码包
```
tar -zxvf nginx-1.12.0.tar.gz
```
5. 配置，编译安装
```
./configure --prefix=/opt/nginx1-12/ #这里不会生成文件夹
make && make install #这一步结束才会正式生成nginx文件
```
6. 启动nginx，进入sbin目录，找到nginx启动命令
```
cd sbin #里面有一个nginx脚本
./nginx #启动
./nginx -s stop #关闭
./nginx -s reload #重新加载nginx配置文件，不重启nginx
./nginx -t #检测nginx.conf语法是否正确

```
### nginx软件目录结构
- conf
    - nginx.conf 修改nginx主配置文件
- sbin
    - nginx 启动命令 ./nginx
- html
    - index.html 存放网页根目录 


### 基于域名的虚拟主机
1. 准备俩域名，在hosts文件中强制解析
    1. 找到windows的hosts文件，强制一个域名解析,地址: C:\windows\system32\drivers\etc\hosts
    ```
    10.0.0.10 www.mytb.com
    10.0.0.10 www/myjd.com
    ```
2. 修改nginx.conf配置文件，写入两个server{} 标签，定义两个虚拟主机
```
    server {
        listen       80;
        server_name  www.mytb.com;
        location / {
            root   /opt/static/mytb;
            index  index.html index.htm;
        }


        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

    }
    server {
        listen 80;
        server_name www.myjd.com;
        location / {
          root   /opt/static/myjd/;
          index   index.html;
          }
}
```
3. 创建虚拟主机定义的网页根目录
```
mkdir -p /opt/static/{mytb,myjd}
```
4. 写入两个网站的index.html文件
```
cd /opt/static/myjd
    touch index.html
cd /opt/static/mytb
    touch index.html
```
5. 重启nginx服务器
```
./nginx -s reload #这是nginx的相对路径
```
6. 在windows中访问自己的两个虚拟主机