### 1.数据库连接：
 - 默认：用户root
 - 创建用户：代码块1
 - 授权：
    - 权限  人

```
常用命令
show databases; 显示数据库
use 数据库名称; 进入数据库
show tables; 显示数据表
select * from 表名; 显示表中的所有列
select name,age,id from 表名; 查看相应列的数据
```
```
创建用户
create user 'alex'@'192.168.1.1' identified by '123123' 指定IP的机器可以登录
create user 'alex'@'192.168.1.%' identified by '123123' 指定前缀的IP可以登录
create user 'alex'@'%' identified by '123123' 任何Ip都可以登录
```
```
给某个用户对某个数据库中的某个表进行授权
grant select,insert,update on db1.* to 'alex'@'%';
grant select,insert,update on db1.t1 to 'alex'@'%'
grant all privileges on db1.t1 to 'alex'@'%';
删除权限
grant all privileges on db1.t1 to 'alex'@'%';
```
```
删除用户
drop user '用户名'@'IP地址';
修改用户
rename user '用户名'@'IP地址'; to '新用户名'@'IP地址';;
修改密码
set password for '用户名'@'IP地址' = Password('新密码')
```
### 2.学习SQL语句规则
 - 操作文件夹
 ```
创建数据库
create database db2;
create database db2 default charset utf8;
显示数据库
show databases;
删除数据库
drop database db2;
```
 - 操作文件
```
show tables;
create table t1(
    列名 类型 null,
    列名 类型 not null,  是否可以为空
    列名 类型 default 1, 默认值
    列名 类型 not null auto_increment primary key, 是否自增
    id int,
    name char(10)) engine=innodb default charset=utf8;
engine=innodb 支持事务，原子性操作
    select * from t1;
    insert into t1(id,name)     values(1,'egon');
清空表
    delete from t1; #清空数据，但id继续增
    truncate table t1; #清空数据，并且id从1开始
删除表
    drop table t2;
```
> auto_increment 自增

> primary key 表示约束（不能重复，且不能为空） 加速查找 
 - 操作文件里的内容
 
```
插入数据
    insert into t1(id,name) values(1,'wunan');
查看数据
    select * from t1;
删除数据
    delete from t1 where id<6;
修改数据
    update t1 set age = 18;
    update t1 set age = 18 where age = 17
```
- 数据类型
    - 数字(无符号:unsign)：
        - tinyint 
        - int
        - bigint
        - float  
        - double 
        - decimal 
            - 存小数比较精确，底层用的字符串
            - 格式：num decimal(10,5)
            - 意义：10为前后的总位数,5为小数点后有几位
    - 字符串
        - char(10)
            - 不管输入的字符串长度等不等于10都占是个位置，查询速度快
        - varchar(10)
            - 只看字符串占用的实际空间，节省空间，但是查询的时候没有char快 
            - mediumtext
            - longtext
        - 上传文件：
            - 文件存硬盘
            - 数据库存路径
    - 时间类型
        - datetime 最重要
    - 枚举类型
        - 例如: size ENUM('x-small','small','large','x-large')
        - insert into shirts(name,size) values('tirst','只能是枚举里的类型')
    - 集合类型
        - 例如:col set('a','b','b')
        - insert into myset(name,col) values(只允许写集合里任意组合)

- 外键：
```
create table userinfo(
    uid bigint auto_increment primary key,
    name varchar(32),
    department_id int
    constraint fk_user_depar foreign key(department_id) references department(id) 
)engine=innodb default charset=utf8

create table department(
    id bigint auto_increment primary key,
    title char(15)
)engine=innodb default charset=utf8,
```

```
create table class(
	cid int not null auto_increment primary key,
	caption varchar(10)
)engine=innodb default charset=utf8;

create table student(
	sid int not null auto_increment primary key,
	sname varchar(10) not null,
	gender varchar(5) not null,
	class_id int,
	constraint stu_cal foreign key (class_id) references class(cid)
)engine=innodb default charset=utf8;

create table teacher (
	tid int not null auto_increment primary key,
	tname varchar(10)
)engine=innodb default charset=utf8;

create table course(
	cid int not null auto_increment primary key,
	cname varchar(10),
	teach_id int,
	constraint cour_tea foreign key(cid) references teacher(tid)
)engine=innodb default charset=utf8;

create table score(
	sid int not null auto_increment primary key,
	student_id int not null,
	corse_id int not null,
	number int not null,
	constraint sc_stu foreign key(student_id) references student(sid),
	constraint sc_tea foreign key(corse_id) references course(cid)
)engine=innodb default charset=utf8;
```

### 三.其它注意点及命令
- 查看表中字段的属性
```
desc 表名
```
- 查看表是如何创建的
```
show create table 表名 \G;  //竖着显示如何创建的表

Table: t2
Create Table: CREATE TABLE `t2` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `num` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8
```
> 其中AUTO_INCREMENT=4是表中没插入一条数据就增加1，并且delete清空表之后会发现该值不会变，所以需要手动修改该值，下次再添加数据时候就会从指定值开始

```
alter table t2 AUTO_INCREMENT=xx;
```

### 设置自增的步长
- Mysql自增的步长基于会话级别（窗口一关就恢复默认了）
- SqlServer自增的步长基于表的级别 
```
基于会话级别的
show session variables like 'auto_inc%';
set session auto_increment_increment=2; 设置步长

基于全局级别的(不建议修改)
show global variables like 'auto_inc%';
set global  auto_increment_increment=2; 设置步长
```

## 四.外键的变种
- 唯一索引
    - 约束不能重复(可以为空)
        - 主键不能重复（不能为空） 
    - 加速查找
```
create table t1(
    id int ...,
    num int,
    xx int,
    unique uq1(num)     单列唯一索引
    unique uq2(num,xxx) 联合唯一索引
)
```

- 外键的变种
- 用户表和部门表
    - 用户：
        - alex      1
        - root      1
        - egon      2
        - laoyao    3
    - 部门：
        - 服务
        - 保安
        - 公关
-  用户表和博客表
    - 用户表：
        - alex
        - root
        - egon
        - laoyao
> 一对多的表
    - 博客表            FK() + 唯一
        - /yuanchenqi/  4
        - /alex3714/    1
        - /xxxxxxx/     1
> 这样就可以保证一个用户对应一个博客表


### 五.Sql语句数据行操作补充
- 增
```
insert into tb(name,age) values('alex',12);
insert into tb(name,age) values('alex',12),('root',18)
insert into tb12(name,age) select name,age from tb11;  //将tb11的数据插入到tb12中
```
- 删
```
delete from tb12;
delete from tb12 where id=2;
delete from tb12 where id>2;
delete from tb12 where id>=2 or name='sunnan';
```
- 改
```
update tb12 set name='alex' where id > 12;
```
- 查
```
select * from tb12;
select id,name from tb12;
select id,name from tb12 where id>10 or name = 'xxx'
select id,name as cname from tb12 where id>12;
```
```
其它：
select * from tb12 where id != 1
select * from tb12 where id in (1,5,12)
select * from tb12 where id not in(1,5,12)
select * from tb12 where id between 5 and 12; 闭区间
select * from tb12 where id in (select id from tb11);
```
```
通配符
%代表匹配多个字符
_匹配一个字符
select * from tb12 where name like "%a"
select * fromm tb12 where name like "a_"
```
```
分页
select * from tb12 limit 2;
select * from tb12 limit 10,10; //第一个10代表从第10条开始去，第二个10代表取10条
select * from tb12 limit 10 offset 20 //代表从第20条开始再取10条数据
```
```
排序
select * from tb12 order by id desc; 从大到小
select * from tb12 order by id asc; 从小到达
```
```
分组
 select count(id) from userinfo5 group by part_id;
 聚合函数
 count,max,min,sum,avg
 ```
 > 如果对于聚合函数结果进行二次删选时，必须使用having
 
 ```
 select count(id),part_id from userinfo5 where id > 0 group by part_id having count(id) > 1 
 ```
 
 - 连表查询
    - 方法一：
 ```
 select * from userinfo5,department5 where userinfo5.id = department5.id
 ```
```
select * from userinfo5 left join department5 on userinfo5.part_id = department5.id; //左边表的数据会全部显示
select * from userinfo5 right join department5 on userinfo5.part_id = department5.id; //右边表的数据会全部显示
```

### 六.转储数据库
```
备份：数据表结构+数据
mysqldump -u root db1 > db1.sql -p
备份：数据表结构
mysqldump -u root db1 -d db1 > db1.sql -p

执行文件：
create database db5;
mysqldump -u root -d db5 < db1.sql -p
```
### 七.其它
```
Mysql中的选择判断语句：
case when min(num) < 10 then 0 else min(num)
解释：如果最小分数小于10分则将该列设置为0否则为最小成
三元运算：
if(sinull(xx),0,1)
左右连表：join
上下连表：union
```
### 八.用户权限管理
#### 基于用户的权限管理
- 参考表结构：
```
用户信息
id  username   pwd     role_id
1     alex     123     1
权限
1     订单管理
2     用户管理
3     bug管理
用户类型&权限
1        1
1        2
2        1
3        1

```
#### 基于角色的权限管理
```
用户信息
id  username   pwd
1     alex     123
权限
1     订单管理
2     用户管理
3     bug管理
角色表：
1       IT部门员工
2       咨询员工
3       IT主管
角色权限管理
 1      1
 1      2
 3      1
 3      2
```
