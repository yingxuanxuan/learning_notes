# MySQL



## 数据库入门



### 概念

* 为什么使用数据库
  * 数据持久化，persistence，把数据保存在可掉电式存储设备中供之后使用
  * 易于管理、结构化查询




* 数据库相关概念
  * DB，database，数据库
    * 狭义数据库指，按特定格式保存数据的文件

  * DBMS，Database Management System，数据库管理系统
    * 是一种操纵和管理数据库的大型软件，用于建立、使用、维护数据库，对数据库进行统一管理和控制，用户通过数据库管理系统访问数据库总表内的数据

  * SQL，Structure Query Language，结构化查询语言
  * DCL，Data Control Language，权限控制
  * DDL，Data Define Language，数据定义语言
  * DML，Data Manipaulation Language，数据操纵语言




### 版本选择

* 纵向版本
  * 主流：Mysql5.7
  * 推广：Mysql8.0.34
  * 版本选择:  <https://zhuanlan.zhihu.com/p/144457223>



* 横向版本
  * MySQL Community Server 社区版本
  * MySQL Enterprise Edition 企业版本
  * MySQL Cluster 集群版本，开源免费
  * MySQL Cluster CGE 高级集群版



### 客户端

* 官方客户端

  * MySQL Workbench，GUI客户端

  * MySQL Workbench OSS，社区版

  * MySQL WorkbenchSE，企业版

* 客户端比较

  * MySQL Workbench
    * 支持数据建模，ER图
  * phpMyAdmin
    * 支持
  * Navicat Preminum
    * 多数据库通用
    * 收费
  * SQLyog
    * 收费
  * dbeaver
    * 需要安装Java



![MySQL :: MySQL Workbench](.gitbook/assets/mysql_wb_performance_dashboard_win.png)



![5 MySQL Workbench Alternatives and Competitors](.gitbook/assets/mysql-workbench-389-2.jpg)



### docker安装

```bash
docker run --name mysql8 -e MYSQL_ROOT_PASSWORD=123456 -d  -p 3306:3306 mysql
```



### Linux rpm安装

```bash
# 下载yum源配置
wget http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm

# 安装yum源
rpm -Uvh *.rpm

# 安装mysql
sudo yum install mysql-community-server
```



### CentOS yum安装

```sh
```



### 查看版本

```sh
# 查看数据库版本

mysql --version;
mysql -V;

# select version();
```



### 启停服务

```bash
# centos6
service mysqld start
service mysqld status
service mysqld stop

# centos7
systemctl start mysqld 
systemctl status mysqld 
systemctl stop mysqld 

# windows
net start mysql80
net stop mysql80
```



### 命令行连接

* `-h`，服务地址，不指定为localhost
* `-P`，服务端口，不指定为3306
* `-u`，登录用户，不指定为root
* `-p`，登录密码，留空为交互式输入，在命令行中输入会产生warning

```bash
mysql -h host -P 3306 -u root -ppasswd

# 退出
exit
# ctrl + c
```



### 远程连接权限配置

```mysql
# < mysql 8.0 允许远程连接
GRANT ALL PRIVILEGES ON *.* TO `root`@`%` IDENTIFIED BY `passwd` with grant option;

# > mysql 8.0 允许远程连接，加密规则改变，通过mysql_native_password恢复
CREATE USER 'root'@'%' IDENTIFIED BY 'passwd'; 
GRANT ALL ON *.* TO 'root'@'%'; 
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'passwd';
FLUSH PRIVILEGES;
```



### 配置文件


```ini
# 配置文件位置
/etc/my.cnf

# 数据文件位置
/var/lib/mysql/

# 修改默认存储位置
[mysqld]
datadir=/var/lib/mysql

# 修改默认日志位置
[mysqld_safe]
log-error=/var/log/mysqld.log

# 修改数据库默认编码
character set atin1;
collation latin1_swedish_ci;
```



### 默认表

* information_schema，表信息，系统信息，权限
* mysql，运行信息
* performance_schema
* sys,



### 字符集配置

* 一个数据表有两个字符相关配置
  * character，或character set，字符集，即支持的编码规则
  * collation，字符之间的排列规则



* 默认字符集

  * mysql5.7以前，默认字符集是`Latin1`，只支持欧洲语言字符集

  * mysql8.0及之后，默认的字符集为`utf8mb4`，支持4字节UTF-8编码的字符集，能够表示世界上几乎所有的字符



* 表和字段的字符集
  * mysql支持给表设置默认字符集和排序规则
  * mysql支持给字段指定字符集和排序规则，但一般不建议这么做
  * 可以修改表的默认字符集并转换所有列，但一般不建议这么做



```mysql
# 显示所有字符集
show character set;

# 显示数据库环境变量
show variables like 'character_set%' / '%char%';
show variables like 'collation%';

# 创建数据库时指定字符集
create database 数据库名 
  default character set utf8
  default collate utf8_general_ci;

# 修改数据库字符集
alter database 数据库名 
  character set utf8 
  collate utf8_general_ci;
  

```



* 运行时修改字符集（不建议）

```mysql
# 修改客户端使用的字符集
set character_set_client=utf8;

# 修改当前连接使用的字符集
set character_set_connection=utf8;

# 修改返回给客户端的结果集的字符集
set character_set_results=utf8;

# 修改当前数据库的默认字符集
set character_set_database=utf8;

# 修改当前数据库的默认校对规则
set collation_database=utf8_general_ci;
```



* 配置文件修改字符集

```ini
# /etc/my.cnf

# utf8mb4 是什么

# utf8扩展
# 5.5.3起支持
# 支持emoji

# 客户端配置
# 或 [mysql]
[client]
default-character-set=utf8mb4

# 服务端配置
[mysqld]
default-storage-engine = INNODB
character-set-server = utf8mb4
collation-server = utf8mb4_general_ci

[mysqld]
init-connect='SET NAMES utf8'
```



### 常用管理命令

```sh
# 设置密码
/user/bin/mysqladmin -u root password 'new-password'

# 安全初始化
mysql_secure_installation

# 创建用户
create user yingxuanxuan identified by "password";

# 删除用户
drop user yingxuanxuan;

# 删除HOST
drop user 'jeffrey'@'localhost';

# 重命名用户
rename user yingxuanxuan to yingxuanxuan2；

# 修改当前用户密码
set password = password("passwd");

# 修改指定用户密码：
set password for username = password("passwd");

# 修改权限

# 权限层级：
# 1.全局
# 2.数据库
# 3.表
# 4.列
# 5.子程序

grant all privileges on 数据库.表 to 用户名@主机 identified by 密码；

# 删除权限
revoke all privileges from username;

# 生效权限
flush privileges;

# 备份
mysqlddump -u root -p 数据库名称 > 备份文件.sql

# 恢复
mysql -u root -p 数据库名称 < 备份文件.sql

# 查看权限
show grants for root@'localhost';

# 列显示查询结果 \G
select * from mysql.user where user='test'and host='127.0.0.1'\G;

# 结果导出到文件：
select * from xxx INTO  OUTFILE 'xxx/xx'

# 查看数据引擎：
show engines;

# 修改数据表引擎
# MyISAM不支持事务，InnoDB支持事务。
alter table person type=INNODB;
```



### 常见查询命令

```mysql
# 查看所有数据库
show databases;

# 创建数据库
create database test;

# 使用数据库
use test;

# 查看所有表
show tables;
show tables from db;

# 查看表结构
desc table;
```



## SQL入门



### 概念

* SQL，Structured Query Language，结构化查询语言
* 是访问和处理数据库的标准计算机语言
* 由IBM上世纪70年代开发出来
* 美国国家标准化组织，ANSI标准
  * SQL-86
  * SQL-89
  * SQL-92
  * SQL-99
* 各厂商数据库的SQL在细节上存在兼容性问题



### 分类

* 根据功能，将SQL语言分为三类，DDL、DML、DCL
* DDL，Data Definition Languages，数据定义语言
  * 定义数据库、表、视图、索引等
    * CREATE
  * 创建、删除、修改数据库和数据表
    * ALTER
    * DROP
    * RENAME
    * TRUNCATE
* DML，Data Manipulation Language，数据操作语言
  * 增删改查
    * INSERT
    * DELETE
    * UPDATE
    * SELECT
* DCL，Data Control Language，数据控制语言
  * 定义数据库、表、字段、用户的访问权限和安全级别
    * GRANT
    * REVOKE
  * 事务提交、回滚
    * COMMIT
    * ROLLBACK
    * SAVEPOINT
* DQL，Data Query Language，数据查询语言
  * 由于数据查询比较重要，查询可能单独拎出来作为DQL
* TCL，Transaction Control Language，事务控制语言
  * COMMIT、ROLLBACK



### 语法规则和规范

* SQL可以卸载一行或多行。为了提高可读性，各子句分行写，必要时使用缩进。
* 命令以`;`或`\g`或`\G`结束
* 关键字不能被缩写也不能分行
* 必须保证所有的括号、单引号、双引号成对
* 必须使用英文半角输入方式
* 字符串类型和日期类型的数据可以使用单引号表示
* 列的别名尽量使用双引号，不建议省略as
* 不区分大小写，建议关键字大写
* 注释
  * `#`开头，单行注释
  * `--`开头，单行注释
  * `/* */`包裹，多行注释 

* 字符和日期必须使用单引号
* 字符串必须使用单引号
* 数字可以使用单引号



## 关系型数据库设计规范

* E-R，entity-relationship，实体-联系模型
  * 实体集，class，对应数据库中的一个表，table
  * 实体，instance，对应数据表中一行，row，也称为记录，record
  * 属性，attribute，对应数据表中一列，column，也称为一个字段，field
  * 联系集
* 表的关联关系，relationship
  * 一对一，one-to-one
  * 一对多，one-to-many
  * 多对多，many-to-many
  * 自关联，self reference



## 数据查询语言，DQL

* DQL, Data Query Language 



### 基础查询 select

```mysql
# 查询所有列
select * from departments;

# 查询指定列（按指定顺序显示）
select department_id,department_name from departments;

# 查询常量值
select 100;
select 'str';

# 查询表达式
select 100 % 98;

# 查询函数
select version();

# 别名
select expression as short; # as
select expression short; # 空格

# 去重 distinct， 仅针对单行
select department_id from departments;
select distinct department_id from employees;

# 字符串拼接
select '100'+90; # 190 其中一个为数值，尝试将字符串转为数值
select 'abc'+90; # 90 转换失败则为0
select null + 90; # null 其中一个为null，则结果为null
select '100' + '90'; # 190
select concat('100', '90', null); # null，其中一个为null则结果null
select concat('100', '90', IFNULL(null, '零');
select CONCAT('100', '90', 80); # '1009080'

# 字段中存在NULL字段会导致字符串拼接后整体为NULL
select concat(`first_name`, ',', `last_name`, ',', ifnull(`commission_pct`, '0')) from employees;

# ifnull
ifnull(var) # 返回1, 0
ifnull(var, 'str') # 返回str，原值
```

### 条件查询 where
```mysql
# 简单条件运算符
< # 小于
> # 大于
= # 等于
<> # 不等于 !=

select * 
from employees 
where salary > 12000;

select last_name, department_id 
from employees 
where department_id <> 90;

# 逻辑运算符
and # &&
or # ||
not # !

select last_name, salary, commission_pct 
from employees 
where salary >= 10000 
and salary <=120000;

select * 
from employees 
where department_id < 90 
or department_id > 110 
or salary > 150000;

select * 
from employees 
where not(department_id >= 90 and department_id <= 110) 
or salary > 150000;

# 模糊查询
like
between and
in
is null
is not null

## like 
% # 任意多个字符
_ # 任意单个字符
\ # 默认转译字符
escape '$' # 自定义转义字符为$

select * from employees where last_name like '%a%';
select last_name, salary from employees where last_name like '____n_l%';
select last_name from employees where last_name like '_\_%';
select last_name from employees where last_name like '_$_%' escape '$';

## like 新版本可以匹配数值型
select * from department_id like '1__';

## between and, 包含临界值，大于等于且小于等于，值不能颠倒
select * from employees where employee_id between 100 and 120;
select * from employees where employee_id not between 100 and 120;

## in，值类型必须统一或兼容 * in中不能带通配符
select last_name, job_id from employees where job_id in('IT_PROT', 'AD_VP', 'AD_PRES');

## is null, is not null, 不能用等号或不等号判断null
select last_name, commission_pct from employees where commission_pct is null;

## <=> 安全等于，可以用于比较null，但是可读性较低
select last_name, commission_pct from employees where commission_pct <=> null;
```

### 排序查询，order by
```mysql
# 语法
select col
from table
[where condition]
order by col [asc|desc]

# 默认asc升序，可以省略不写

## 降序
select * from employees order by salary desc;

## 升序
select * from employees order by salary;

## 使用别名排序
select *, salary*12*(1+ifnull(commission_pct, 0)) as S 
from employees 
order by salary*12*(1+ifnull(commission_pct, 0)) desc;

select *, salary*12*(1+ifnull(commission_pct, 0)) as S 
from employees 
order by S desc;

## 字符长度 length() 
select * from employees order by lenght(last_name);

## 多个字段排序
select * from employees order by salary asc, employee_id desc;
```

### 常见单行函数
* 格式：select func(var) [from table]; # select是否用到table中的字段

```mysql
# 字符函数

## length() # 字符串长度
length('e') # 英文字符占1个字节
length('中') # 中文占3个字节
show variables like '%char%'; # utf8

## concat() # 拼接字符串
select concat(last_name, '_', first_name)

## upper() lower() # 大小写转换
select UPPER('abc');
select lower('ABC');

## substr() substring() # 截取字符串
select substr('12345678', 7); # 78，从1开始计数截取到结束
select substr('12345678', 2，3); # 234，从2开始截取3个字符

## instr() # 查询子串位置
select instr('12345678', '678'); # 6
select instr('12345678', '90'); # 找不到返回0

## trim() # 清除空格或指定字符
select trim('    123    '); # 123
select trim('a' from 'aaaa123aaaa') # 123

## lpad(), rpad() # 左右填充
select lpad('a', 3, '*'); # **a
select rpad('a', 3, '*'); # a**

## replace() 替换
select replace('abc****abc*****abc', 'abc', '123'); # 所有都会被替换


# 数学函数

## round() 四舍五入，仅考虑1位小数
select round(1.500001); # 2
select round(1.499999); # 1

## ceil() 向上取证，大于等于该参数的最小整数
select cell(1.002) # 2
select cell(1.000) # 1
select cell(-1.02) # -1

## floor() 向下取整
select floor(0.9); # 0
select floor(-0.8); # -1

## truncate() 截断，小数点后保留n位
select truncate(1.1234, 1) # 1.1

## mod() 取余
select mod(10, -3); # 1
select mod(-10, -3); # -1

## rand() 0-1随机小数


# 日期函数

## now() 当前日期时间
select now(); # 2020-06-10 22:41:13

## curdate() 当前日期
select curdate(); # 2020-06-10

## curtime() 当前时间
select curtime(); # 22:41:13

## year() month() monthname() day() hour() minute() second()
select year(now());
select year('2008-1-3');
select month(now());

## str_to_date() # 字符串转换日期

%Y # 四位年
%y # 2位年
%m # 2位月
%c # 1-2位月
%d # 日
%H # 2位小时
%h # 1-2位小时
%i
%s # 秒
select str_to_date('1998-3-2', '%Y-%c-%d');

## date_format() # 日期转字符串


## datediff() # 日期差
select datediff(now(), '1995-1-1');

# 其他函数
select version();
select database();
select user(); # root
select password('word') # 字符加密
select md5('word')

# 流程控制函数

## if函数
select if(10>5, 'large', 'small');
select if(commission_pct is null, 'no', 'yes');

## case，switch case
case 字段或表达式
when 常量1 then 值或者语句1;
when 常量2 then 值或者语句2;
else 值或者语句2;
end;

select salary, department_Id,
case department_id
when 30 then salary*1.1
when 40 then salary*1.2
when 50 then salary*1.3
else salary
end as new_salary
from employees;

## case，多重if
case
when 条件1 then 值或者语句1
when 条件2 then 值或者语句2
...
else  值或者语句2
end;

select salary,
case
when salary>20000 then 'A'
when salary>15000 then 'B'
when salary>10000 then 'C'
else 'D'
end as s
from employees;

```

### 聚合函数

* 一般结合group by分组后使用
* 又称分组函数，统计函数，组函数

#### 求和，sum

```mysql
# sum，支持数值类型，null不参与运算
select sum(salary) from employees;
```

#### 平均值，agv

```mysql
# avg，支持数值类型，null忽略，平均除数也忽略
select avg(salary) from employees;
```

#### 最大值，max

```mysql
# max，支持字符型和日期型，可以比较大小就支持，忽略null值
select max(salary) from employees;
```

#### 最小值，min

```mysql
# min
select min(salary) from employees;
```

#### 计数，count

```mysql
# 不指定列计数
select count(*) from employees; # MYISAN 效率最高
select count(1) from employees; # MYISAN INNODB 效率相同

# 指定列，忽略null值计数
select count(salary) from employees; # 判断是否为NULL
```

#### 结合去重，distinct

```mysql
# distinct
select count(distinct salary) from employees;
select sum(distinct salary) from employees;
```

#### 一次查询多次使用

```mysql
select sum(salary), avg(salary), max(salary), min(salary), count(salary) 
from employees;
```

### 分组查询，group by

* 一般结合聚合函数使用

#### 按分组函数分组

```mysql
# 格式
select group_function()
from table
[where condition]
[group by group_by_expression]
[order by ]

# 每个工种最高工资
select max(salary), job_id 
from employees 
group by job_id;

# 每个位置部门个数
select count(*), location_id 
from departments 
group by location_id;

# 每个部门的平均工资
select AVG(salary), department_id 
from employees 
group by department_id;

# 有奖金的每个领导手下的员工的最高工资
select max(salary), manager_id 
from employees 
where commission_pct is not null 
group by manager_id;
```

select的字段必须在group by字段之内，否则没有意义

```mysql
# 错误语句，一个分组内无法选出一个有意义的id值
# select id from tt group by name, age;

# 正确语句
select min(id) from tt group by name, age;
```



#### 过滤分组后的信息，having

```mysql
## 哪个部门的员工个数大于2

## 临时表
select count(*), department_id 
from employees 
group by department_id;

## having 条件筛选组
select count(*), department_id 
from employees 
group by department_id
having count(*) > 2;

## 查询每个工种有奖金的员工的最高工资大于12000的工种编号和最高工资
select max(salary) as max_salary , job_id 
from employees 
where commission_pct is not null 
group by job_id 
having max_salary > 12000;

## having子句
从原始表中筛选放在where里
从生成表中筛选放在having里
```

#### 按表达式或函数分组

```mysql
# 查询名字长度分组，查询员工的个数，筛选个数大于5的组
select count(*), length(last_name) 
from employees 
group by length(last_name) 
having count(*) > 5;
```

#### 按多个字段分组
```mysql
# 查询每个部门每个工种的员工的平均工资
select avg(salary), department_id, job_id 
from employees 
group by job_id, department_id;
```

### sql92 连接查询，多表查询，多表连接
* 多表查询必须添加限定条件
* 92标准仅支持内连接
* 内连接：
    *  等值连接
        *  多表等值连接结果为多表的交集部分
        *  n表连接至少需要n-1个连接条件
        *  多表连接顺序没有要求
        *  一般需要为表起别名
    *  非等值连接
    *  自连接
* 外连接：
    *  左外连接
    *  右外连接
    *  全外连接
* 交叉连接

#### 等值连接
```mysql
select beauty.id, beauty.name, boys.boyName 
from beauty, boys 
where beauty.boyfriend_id = boys.id;

select last_name, department_name 
from employees,departments 
where employees.department_id = departments.department_id;

# 为表起别名，提高简洁度，去除歧义
select e.last_name, e.job_id, j.job_title 
from employees as e, jobs as j 
where e.job_id = j.job_id

# 等值连接加筛选
select e.last_name, d.department_name 
from employees as e, departments as d 
where e.department_id = d.department_id 
and e.commission_pct is not null;

select department_name, city
from departments as d, locations as l
where d.location_id = l.location_id 
and city like '_o%';

# 等值连接加分组
select count(*), city
from departments as d, locations as l
where d.location_id = l.location_id
group by city;

select d.department_name, d.manager_id, min(salary)
from departments as d, employees as e
where d.department_id = e.department_id
and commission_pct is not null
group by d.department_name, d.manager_id;

# 等值连接加排序
select job_title, count(*)
from employees as e, jobs as j
where e.job_id = j.job_id
group by job_title
order by count(*) desc;

# 三表连接
select last_name, department_name, city
from employees as e, departments as d, locations l
where e.department_id=d.department_id
and d.location_id=l.location_id
and city like 's%';
```

#### 非等值连接
```mysql
# 员工的工资和工资级别
select salary, grade_level
from employees as e, job_grades as g
where salary between g.lowest_sal and g.highest_sal;
```

#### 自连接
```mysql
# 员工和领导名称，领导本身在员工表中

select e.employee_id, e.last_name, e.manager_id, m.last_name as manager_last_name
from employees as e, employees as m
where e.manager_id = m.employee_id;
```

### sql99
* 内连接 inner join
* 外连接：一个表中有，另一个表中没有
* 左外连接 left [outer] join，left左边是主表，主表全显示
* 右外连接 right [outer] join，right左边是辅表，主表全显示
* 全外连接 full [outer] join
* 交叉连接 cross
```mysql
# 格式
select 查询列表
from 表1 别名 
连接类型 join 表2 别名
on 连接条件
[where]
[group by]
[having]
[order by]
```

#### 内连接-等值连接
```mysql
select last_name, department_name
from employees as e
inner join departments d
on e.department_id = d.department_id;

select last_name, job_title
from employees as e
inner join jobs as j
on e.job_id = j.job_id
where e.last_name like '%e%';

select city, count(*)
from departments as d
inner join locations as l
on d.location_id = l.location_id
group by city
having count(*) > 3;

select d.department_name, count(*)
from employees as e
inner join departments as d
on e.department_id = d.department_id
group by e.department_id
order by count(*) desc;

select last_name, department_name, job_title
from employees as e
inner join departments as d on e.department_id= d.department_id
inner join jobs as j on e.job_id = j.job_id
order by department_name desc;
```

#### 内连接-非等值连接
```mysql
select salary, grade_level
from employees as e
inner join job_grades as g
on e.salary between g.lowest_sal and g.highest_sal;

select count(*), grade_level
from employees as e
join job_grades as g
on e.salary between d.lowest_sal and g.highest_sal
group by grade_level
having count(*) > 20
order by grade_level desc;
```

#### 内连接-自连接
```mysql
select e.last_name, m.last_name
from employees as e
inner join employees as m
on e.manager_id = m.employee_id;

select e.last_name, m.last_name
from employees as e
inner join employees as m
on e.manager_id = m.employee_id
where e.last_name like '%k%';
```

#### 外连接
```mysql
select *
from beauty as b
left outer join boys as bo
on b.boyfriend_id = bo.id;

select *
from beauty as b
left outer join boys as bo
on b.boyfriend_id = bo.id
where bo.id is null;

select *
from boys as bo
left outer join beauty as b
on b.boyfriend_id = bo.id
where b.id is null;

select d.*
from departments as d
left outer join employees as e
on d.department_id = e.department_id
where e.employee_id is null;
```

#### 全外连接 - 不支持
```mysql
select b.*, bo.*
from beauty as b
full outer join boys as bo
on b.boyfriend_id = bo.id;
```

#### 交叉连接 - 笛卡尔乘积
```mysql
select b.*, bo.*
from beauty as b
cross join boys as bo;
```


### 子查询
* 按出现位置分类
    * select后：仅支持标量子查询
    * from后：支持表子查询
    * where和having后：
        * 标量子查询（单行），搭配 >, <, =, <>, >=, <=使用
        * 列子查询（多行），搭配 in/not in, any/some, all使用
        * 行子查询
    * exists后（相关子查询）：支持表子查询
* 按结果集的行列数不同
    * 标量子查询（结果集只有一行一列）
    * 列子查询（结果集只有一列多行）
    * 行子查询（结果集有一行多列）
    * 表子查询（结果集多行多列）

#### 标量子查询
```mysql
select *
from employees as e
where salary > (
  select salary
  from employees
  where last_name = 'Abel'
)

select last_name, job_id, salary
from employees
where job_id = (
  select job_id
  from employees
  where employee_id = 141
) and salary > (
  select salary
  from employees
  where employee_id = 143
);

select *
from employees
where salary=(
  select min(salary)
  from employees
);

select min(salary), department_id
from employees
group by department_id
having min(salary) > (
  select min(salary)
  from employees
  where department_id = 50
);


```

#### 列子查询（多行子查询）
```mysql
select last_name
from employees
where department_id in (
  select distinct department_id
  from departments
  where location_id in (1400, 1700)
);


select last_name, employee_Id, job_id, salary
from employees
where salary < any (
  select max(salary)
  from employees
  where job_id = 'IT_PROG'
) and job_id <> 'IT_PROG';
```

#### 行子查询（一行多列，或者多行多列）
* 一般不用，有其他等价形式
```mysql
select *
from employees
where (employee_id, salary) = (
  select min(employee_id), max(salary)
  from employees
);

# 等价子查询
select *
from employees
where employee_id = (
  select min(employee_id)
  from employees
) and salary = (
  select max(salary)
  from employees
);
```

#### select子查询，子查询必须一行一列
```mysql
select d.*,(
  select count(*)
  from employees as e
  where e.department_id = d.department_id)
from departments as d;


select (
  select department_name
  from departments as d
  inner join employees as e
  on d.department_id = e.department_id
  where e.employee_id = 102
) as d;
```

#### from子查询
```mysql
select avg_dep.*, g.grade_level
from (
  select avg(salary) as avg_sal, department_id
  from employees
  group by department_id
) as avg_dep
inner join job_grades as g
on avg_dep.avg_sal between lowest_sal and highest_sal;

```

#### exists子查询，相关子查询
```mysql
# 有值则显示1
select exists (
  select employee_id from employees
);

# 无值显示0
select exists (
  select employee_id from employees where employee_id = 30000
);

# 相关子查询
select department_name
from departments as d
where exists(
  select *
  from employees as e
  where e.department_id = d.department_id
);

# 等价于where in子查询
select department_name
from departments as d
where d.department_id in (
  select distinct department_id
  from employees
);
```

### 分页查询

语法

```mysql
select 查询列表
from 表
join type 表 on 条件
where 条件
group by 条件
having 条件
order by 排序字段
limit offset, size # 索引从0开始
# limit (page - 1)*size, size

执行顺序 from -> where -> group -> having -> select -> order by -> limit
```

示例

```mysql
select * from employees limit 0, 5;
select * from employees limit 5; # offset省略

select * from employees limit 10, 15;

select *
from employees
where commission_pct is not null
order by salary desc
limit 10;
```

### 联合查询，合并查询，union

* 多条处查询结果合并

* 合并不同表
* 可以拆分
* 列数必须相同
* union # 去重联合
* union all # 不去重联合
```mysql
# 拆分前
select * from employees where email like '%a%' or department_id > 90;

# 拆分后
(select * from employees where email like '%a%') union
(select * from employees where department_id > 90);
```

## 数据操纵语言，DML

* Data Manipaulation Language 

### 插入，insert
* 可以为空的字段可以不写
* 有默认值的字段可以不写
* 列的顺序可以颠倒

```mysql
# 格式1
insert into 表名(列1，列1) values(值1, 值2);

# 格式1，省略字段（按定义顺序）
insert into 表名 values (值1, 值2)

# 单行插入示例
insert into beauty(id, name, sex, borndate, phone, photo, boyfriend_id)
value(13, 'tangyixing', 'f', '1990-4-23', '18999999999', null, 2);

# 多行插入示例
insert into beauty(id, name, sex, borndate, phone, photo, boyfriend_id)
value(14, 'tangyixing', 'f', '1990-4-23', '18999999999', null, 2),
value(15, 'tangyixing', 'f', '1990-4-23', '18999999999', null, 2),
value(16, 'tangyixing', 'f', '1990-4-23', '18999999999', null, 2);

# 插入结合子查询
insert into beauty(id, NAME, phone)
select 26, 'songqian', '188';

# 插入多行结合子查询
insert into beauty
select 14, 'tangyixing', 'f', '1990-4-23', '18999999999', null, 2 union
select 15, 'tangyixing', 'f', '1990-4-23', '18999999999', null, 2 union
select 16, 'tangyixing', 'f', '1990-4-23', '18999999999', null, 2;

# 格式2
insert into 表名
set 列1=值, 列2=值, ...

# 插入示例
insert into beauty
set id=19, name='liutao', phone = '999';

```

### 修改，update

* 不加条件是全表修改
* 加上条件是匹配的修改

```mysql
# 修改单表
update 表名
set 列=值, 列=值, ...
where 筛选条件

update beauty
set phone='13888888888'
where name='tangyixing';

# 修改多表
## 92语法
update 表1 别名, 表2 别名
set 列=值
where 连接条件
and 筛选条件

## 99语法
update 表1 别名
inner | left | right join 表2 别名
on 连接条件
set 列=值
where 条件

update boys as bo
inner join beauty as b
on bo.id = b.boyfriend_id
set b.phone = '114'
where bo.boyname = '张无忌';

update boys as bo
right join beauty as b
on bo.id=b.boyfriend_id
set b.boyfriend_id=2
where bo.id is null;
```

### 删除，delete
* 不加条件是全表删除
* 加上条件是匹配的删除
```mysql
# 方法1
delete from 表名
where 条件;

## 单表删除
delete from beauty 
where phone like '%9';

## 多表删除，级联删除
delete from 表1，表2
where 条件;


## 限制单条删除
delete from beauty 
where phone like '%9'
limit 1

### sql92
delete 别名
from 表1, 表2
where 连接条件
and 筛选条件;

### sql99
delete 表1, 表2
from 表1
inner | left | right | join 表2
on 连接条件
where 筛选条件;

```

### 清空，truncate

* truncate和delete的区别
  * truncate 清空数据表，不能加条件
  * truncate效率高
  * truncate可以置零自增长的值
  * truncate删除没有返回值
  * truncate删除不能回滚，delete可以回滚

```
truncate table 表名；
```

### 删除、修改表时，不允许从表内查询数据

```mysql
# 错误
# delete from tt where id not in (select min(id) from tt group by name, age);

# 正确，先将子查询保存成临时表t
delete from tt where id not in (select id from (select min(id) as id from tt group by name, age) as t)
```

## 数据定义语言，DDL

* DDL, Data Define Language

### 数据库管理

#### 创建数据库

```mysql
create database 数据库名称;
create database if not exists 数据库名称;
```

#### 删除数据库

```mysql
drop database name;
drop database if exists name;
```

#### 修改数据库

```mysql
# 修改数据库字符集
alter database name character set utf8mb4;

# 修改数据库（可能造成问题）
# rename database old to new; 
# 直接修改文件夹名称
```

### 数据表管理

#### 创建表

```mysql
# 创建表前检查表是否存在
create table if not exists ...;

# 创建表格式
create table 表名
(
    列名 类型(长度) 约束,
    列名 类型(长度) 约束,
    列名 类型(长度) 约束
);

# 示例

# 注意：创建表前创建数据库
create database test;
# 注意：创建表前切换到数据库
use test;
create table if not exists book (
  id int,
  bname varchar(20),
  price double,
  authorId int,
  publishdate datetime
);

# 查看表定义
desc book;

create table author (
  id int,
  au_name varchar(20),
  nation varchar(10)
);

desc author;
```

#### 查看表定义

```mysql
desc 表名;
```

<div align="left">
<figure><img src=".gitbook/assets/image-20230808173523452.png" alt=""><figcaption></figcaption></figure>
</div>

#### 修改表

```mysql
# 修改表
alter talbe 表名 add | drop | modify | change column 列名 类型 约束;

# 修改列名
alter table book change column old_col new_col [new_type];

# 修改列的类型或约束
alter talbe book modify column pubdate timestamp;

# 添加新列，默认末尾
alter talbe table_name add column new_col_name double;

# 添加新列到指定位置
alter talbe table_name add column new_col_name double [first|after 列名];

# 删除列
alter table author drop column annual;

# 修改表名
alter table old_table_name rename [to] new_table_name;
```

#### 删除表

```mysql
drop table book_author;
drop table if exists table_name;
```

#### 复制表

```mysql
# 只复制表结构
create table copy_table like raw_table;

# 复制表结构+部分内容
create table copy_table
select * from author
where 条件;

# 只复制部分列
create table copy_table
select 列1, 列2
from raw_table;
where 0;
```

### 常见数据类型

* 数值型
    * 整形
    * 小数
        * 浮点型
        * 定点型
* 字符型
    * 短文本：char、varchar
    * 长文本：text、blob（二进制）
* 日期型

#### 整形 
```mysql
Tinyint # 1字节
Smallint # 2字节
Mediumint # 3字节
Int、Integer # 4字节
Bigint # 8字节

create table tab_int(
    t1 int(8) zerofill,
    t2 int unsigned
);
```
* 超出的值取临界值
* int(8) zerofill，8为默认长度，不足8位填充补0


#### 浮点型
```mysql
float(M, D) # 4字节
double(M, D) # 8字节
DEC(M, D) # M+2字节， 默认精度是 DEC(10, 0)
DECIMAL(M, D) 
```

* M: 整数部分+小数部分的位数，超出的话取整数最大+小数最大
* D：小数部分的位数

#### 字符型
* 较短的文本char(最长字符数）、varchar（最长字符数）  
* 短二进制binary、varbinary
* 较长的文本text、blob

| 类型    | 写法       | M的意思    | 特点     | 空间耗费 | 效率   |
| ------- | ---------- | ---------- | -------- | -------- | ------ |
| char    | char(M/1)  | 最大字符数 | 固定长度 | 浪费空间 | 效率高 |
| varchar | varchar(M) | 最大字符数 | 可变长度 | 节省空间 | 效率低 |


#### 枚举类型
```mysql
create table tab_char (
    c1 enum('a', 'b', 'c')
);

insert into tab_char values('a');
insert into tab_char values('A'); # 自动转换为小写
insert into tab_char values('m'); # 超出
```

#### set类型
```mysql
create table tab_set (
    c1 set('a', 'b', 'c', 'd');
);

insert into tab_set values('a');
insert into tab_set values('A,B')
insert into tab_set values('a,c,d');
```

#### 日期
| 类型      | 空间            | 最小值              | 最大值              |
| --------- | --------------- | ------------------- | ------------------- |
| date      | 4字节，只有日期 | 1000-01-01          | 9999-12-31          |
| datetime  | 8字节           | 1000-01-01 00:00:00 | 9999-12-31 23:59:59 |
| time      | 3字节，只有时间 | -838:59:59          | 838:59:59           |
| timestamp | 4字节           | 19700101080001      | 2038（受时区影响）  |
| year      | 1字节           | 1901                | 2155                |

```mysql
CREATE TABLE tab_data(
	t1 DATETIME,
	t2 TIMESTAMP
);

INSERT INTO tab_data VALUES(NOW(), NOW());
SELECT * FROM tab_data;
SHOW VARIABLES LIKE 'time_zone';
SET time_zone = '+9:00';
```

### 约束
* not null # 非空，字段不能为空
* default # 默认值
* primary key # 主键约束，非空，唯一
* unique # 唯一
* check # 检查约束，mysql不支持
* foreign key # 外键，限制两个表的关系


| 类型        | 唯一性 | 是否可以为空 | 一个表中可以有多少个 | 是否允许组合 |
| ----------- | ------ | ------------ | -------------------- | ------------ |
| primary key | 是     | 否           | 1个                  | 允许，不推荐 |
| unique      | 是     | 是           | 多个                 | 允许，不推荐 |

#### 列级约束
* foreign key references约束无效
```mysql
CREATE TABLE stuinfo(
	id INT PRIMARY KEY,
	stuName VARCHAR(20) NOT NULL,
	gender CHAR(1) CHECK(gender='男' OR gender='女'),
	seat INT UNIQUE,
	age INT DEFAULT 18,
	majorId INT REFERENCES major(id)
);

CREATE TABLE major(
	id INT PRIMARY KEY,
	majorName VARCHAR(20)
);
```


#### 表级约束
* 一般只写foreign key约束
* foreign key在主表中必须为主键
* 先建立主表，再建立从表
* 先插入主表，再建立从表
* 先删除从表，在删除主表
```mysql
# 命名
CREATE TABLE stuinfo(
	id INT,
	stuname VARCHAR(20),
	gender CHAR(1),
	seat INT,
	age INT,
	majorid INT,
	CONSTRAINT pk PRIMARY KEY(id),
	CONSTRAINT uq UNIQUE(seat),
	CONSTRAINT ck CHECK(gender='男' OR gender='女'),
	CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorid) REFERENCES major(id)
);

# 匿名（系统自动生成）
CREATE TABLE stuinfo(
	id INT,
	stuname VARCHAR(20),
	gender CHAR(1),
	seat INT,
	age INT,
	majorid INT,
	PRIMARY KEY(id),
	UNIQUE(seat),
	CHECK(gender='男' OR gender='女'),
	FOREIGN KEY(majorid) REFERENCES major(id)
);
```

#### 修改表时添加约束
```mysql
CREATE TABLE stuinfo(
	id INT,
	stuname VARCHAR(20),
	gender CHAR(1),
	seat INT,
	age INT,
	majorid INT
);

# 添加约束
ALTER TABLE stuinfo MODIFY COLUMN stuname VARCHAR(20) NOT NULL;
ALTER TABLE stuinfo MODIFY COLUMN age INT DEFAULT 18;
ALTER TABLE stuinfo MODIFY COLUMN id INT PRIMARY KEY;
ALTER TABLE stuinfo ADD PRIMARY KEY(id);
ALTER TABLE stuinfo MODIFY COLUMN seat INT UNIQUE;
ALTER TABLE stuinfo ADD UNIQUE(seat);
ALTER TABLE stuinfo ADD CONSTRAINT fk_stuinfo_majorid FOREIGN KEY(majorid) REFERENCES major(id);

# 删除约束
ALTER TABLE stuinfo MODIFY COLUMN stuname VARCHAR(20) NULL;
ALTER TABLE stuinfo MODIFY COLUMN age INT;
ALTER TABLE stuinfo DROP PRIMARY KEY;
ALTER TABLE stuinfo DROP INDEX seat;
ALTER TABLE stuinfo DROP FOREIGN KEY;
```

### 标识列，自增长列, AUTO_INCREMENT
* 一个表只能有一个自增长列
* 自增长列必须是key
* 类型只能是数值型，int，float，double

```mysql
# 创建表时设置
CREATE TABLE tab_identity(
	id INT PRIMARY KEY AUTO_INCREMENT;
	NAME VARCHAR(20)
);

# 插入方式
INSERT INTO tab_identity VALUES(NULL, 'john');
INSERT INTO tab_identity(NAME) VALUES('lucy');

# 修改步长
SHOW VARIABLES LIKE '%auto_increment%';
SET auto_increment_increment=3;
```



## 事务控制语言，TCL

* TCL，Transaction Contral Language



### 存储引擎
* innodb
* myisam # 5.5之前，不支持事务
* memory

```mysql
# 显示所有支持存储引擎
SHOW ENGINES;
```

### 事务属性ACID，四大特性
* Atomicity，原子性，不可再分，都执行或者都不执行
* Consistency，一致性，数据从一个一致状态切换到另一个一致状态，准确、完整、可靠
* Isolation，隔离性，事务不受其他事务干扰
* Durability，持久性，事务操作永久改变数据库数据

### 事务创建
* 隐式事务，没有明显开始和结束标记，如insert、update、delete
    * `show variable like 'atocommit'`，默认每条语句都是一条事务
* 显式事务，有明显的结束和开启标记
    * `set autocommit=0`
    * `start transaction`(可选)
    * `commit;` # 提交 
* `rollback;` # 回滚
* `savepoint;` # 设置保存点

```mysql
SET autocommit=0;
START TRANSACTION;
UPDATE account SET balance = 500 WHERE username='A';
UPDATE account SET balance = 1500 WHERE username='B';
COMMIT;


set autocommit=0;
start transaction;
delete ...;
savepoint a;   # 设置回滚点
delete ...;  
rollback to a; # 回滚到保存点
```

### 多事务并发,隔离级别
* 脏读        # 读到其他事务没有被提交的数据
* 不可重复读  # 读取后数据被更新
* 幻读        # 读到其他数据未提交的新插入数据


* read-uncommited   # 可能造成脏读，不可重复读，幻读
* read-commited     # 可能造成不可重复读，幻读
* repeatedable-read # 可能造成幻读
* serializable      # 数据一致


* mysql默认repeatedable read
* oracle支持read commited和serializable，默认为read commited

```mysql
# 查看隔离级别
select @@tx_isolation;

# 设置当前session隔离界别
set session transaction isolation level read uncommited;

# 设置全局隔离级别
set global transaction isolation level read commited;
```

### 事务delete、truncate区别
* delete可以回滚
* trancate不能回滚

## 视图，虚拟表

* mysql5.1版本新增  
* 重用sql语句
* 简化sql查询
* 保护（隐藏数据）


* 功能示例
```mysql
# 原版查询
select stuname, majorname
from stuinfo as s
inner join major as m 
on s.majorid = m.id
where s.stuname like 'xx';

# 创建视图
create view v1
as 
select stuname, majorname
from stuinfo as s
inner join major as 
on s.majorid = m.id;
# 从视图查询
select * from v1 where stuname like 'xx';

# 修改视图1
create or replace view name
as
select ...;

# 修改视图2
alter view name
as 
select ..;

# 删除视图
drop view v1, v2, r3;

# 查看视图
desc v1;
show create view v1;

```

### 更新视图中的数据
1. 默认可以更新到原始表
2. 含有分组函数、distinct、group by、having、union、常量、join、where子表、涉及多表都不能更新。
3. 组合字段不能更新
4. 字段不含原表必选项等不能更新
5. 一般不使用

## 变量
* 系统变量
    * 全局变量，数据库全局
    * 会话变量，当前连接
* 自定义变量
    * 用户变量，session中有效
    * 局部变量，begin end中有效

### 系统变量
```mysql
show variables; # 默认session变量
show global variables;  # 全局变量
show session variables; # 会话变量
show variable like '%xxx%';
show global variable like '%xxx%';

# 查询具体变量
select @@系统变量名;
select @@global.系统变量名;

set global 系统变量名 = 值;
set @@global.系统变量名 = 值;
```

### 自定义变量-用户变量
```mysql
# 声明并初始化
set @var=val;
set @var:=val;
select @var:=val;
select col into var from table;

# 查看值
select @var


set @m=1;
set @n=2;
set @sum=@m+@n;
select @sum;

```

### 自定义变量-局部变量
```mysql
# 声明
declare var value;

# 声明并赋值
set var=value;
set var=:value;
select @var:=value;
select col into var from table;

# 查看
select var
```

## 存储过程
* 提高重用性
* 简化操作
* 提高性能，预编译

### 创建
* 参数格式：参数模式 参数名 参数类型
* 参数模式：IN / OUT / INOUT
* BEGIN END：只有一句语句则可以省略BEGIN END
* 存储过程体中必须分号结束
* DELIMITER $ 设置结束标记
* CALL调用

```mysql
CREATE PROCEDURE pro_name(var) 
BEGIN
	expression;
END
```

#### 空参列表
```mysql
DELIMITER $

CREATE PROCEDURE mypl1()
	INSERT INTO admin(usernmae, `password`)
	VALUES('john1', '0000'), ('lily', '0000');
END $

CALL myp1()$
```

#### IN模式参数
```mysql
DELIMITER $

CREATE PROCEDURE myp2(IN beautyName VARCHAR(20))
BEGIN
	SELECT bo.*
	FROM boys AS bo
	RIGHT JOIN beauty AS b
	ON bo.id = b.boyfriend_id
	WHERE b.name= beautyName;
END $

CALL myp2('柳岩') $


CREATE PROCEDURE myp3(IN username VARCHAR(20), IN PASSWORD VARCHAR(20))
BEGIN
	DECLARE result VARCHAR(20) DEFAULT '';
	
	SELECT COUNT(*) INTO result
	FROM admin
	WHERE admin.username = username
	AND admin.passwd = PASSWORD;
	
	SELECT IF(result>0, 'success', 'fail');
END $

CALL myp3('name', 'passwd') $
```

#### out模式参数
```mysql
CREATE PROCEDURE myp4(IN beautyName VARCHAR(20), OUT boyname VARCHAR(20))
BEGIN
	SELECT bo.boyName INTO boyName
	FROM boys AS bo
	INNER JOIN beauty AS b
	ON bo.id=b.boyfriend_id
	WHERE b.name = beautyName;
END $

SET @bname$
CALL myp4('xxx', @bname) $
SELECT  @bname$
```


#### inout模式参数
```mysql
a,b翻倍并返回

CREATE PROCEDURE myp8(INOUT a INT, INOUT b INT)
BEGIN
	SET a = a*2;
	SET b = b*2;
END $

SET @m=10$
SET @n=20$
CALL myp8(@m, @n)$
```

### 删除，一次只能删除一个
```mysql
drop procedcure mypname;
```

### 查看存储过程
```mysql
show create procedure mypname;
```

## 函数
* 存储过程可以不返回值
* 函数只能有一个返回值
* 函数必须有return语句
* 函数参数不需要IN/OUT/INOUT

### 创建
```mysql
create function 函数名(参数列表) returns 返回类型
begin
    函数体
end
```

### 调用
```mysql
select 函数名(参数列表)
```

### 查看
```mysql
show create function myf3;
```

### 删除
```mysql
drop function myf3;
```

### 示例
```mysql
CREATE FUNCTION myf1() RETURNS INT
BEGIN
	DECLARE c INT DEFAULT 0;

	SELECT COUNT(*) INTO c
	FROM employees;
	RETURN c;
END $

SELECT myf1()$
```


## 流程控制结构
### if函数
```mysql
if (表达式1， 表达式2， 表达式3)
如果表达式1城里，返回表达式2的值，否则返回表达式3的值
```

### case

#### 值判断
```mysql
case 变量|表达式|字段
when 要判断的值 then 返回的值
when 要判断的值 then 返回的值
when 要判断的值 then 返回的值
else 返回值n
end
```

```mysql
case 变量|表达式|字段
when 要判断的值 then 语句;
when 要判断的值 then 语句;
when 要判断的值 then 语句;
else 语句;
end case;
```

#### 条件判断
```mysql
case
when 条件 then 返回
when 条件 then 返回
when 条件 then 返回
else 返回值n
end
```

```mysql
case
when 条件 then 语句;
when 条件 then 语句;
when 条件 then 语句;
else 语句n;
end case;w
```

### if结构（只能用在BEGIN END中）
```mysql
if 条件1 then 语句1;
elseif 条件2 then 语句2;
else 语句n
end if;
```

### 循环
* while loop repeat
* iterate类似于continue
* leave类似于break

```mysql
while 循环条件 do
    语句;
end while;

标签:while 循环条件 do
    语句;
end while 标签;
```

```mysql
标签:loop
    语句
end loop 标签;
```

```mysql
标签:repeat

until 结束循环的条件
end repeat 标签;
```

## mysql架构介绍
### mysql介绍
* mysql内核
* sql优化工程师
* mysql服务器的优化
* 各种参数常量设定
* 查询优化语句
* 主从复制
* 软硬件升级
* 容灾备份
* sql编程

### mysql linux安装
```mysql
# 检查是否安装
rpm -qa | grep mysql

# rpm安装
yum install autoconf # 解决 perl 依赖
sudo rpm -ivh MySQL-server-5.6.48-1.el7.x86_64.rpm --force --nodeps
sudo rpm -ivh MySQL-client-5.6.48-1.el7.x86_64.rpm --force --nodeps

yum erase postfix
yum erase mariadb-libs
sudo rpm -ivh mysql-community-common-8.0.20-1.el7.x86_64.rpm
sudo rpm -ivh mysql-community-libs-8.0.20-1.el7.x86_64.rpm
sudo rpm -ivh mysql-community-client-8.0.20-1.el7.x86_64.rpm
sudo rpm -ivh mysql-community-server-8.0.20-1.el7.x86_64.rpm

# repo安装（慢）
wget https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
rpm -ivh mysql80-community-release-el7-3.noarch.rpm
yum install mysql-community-client mysql-community-server

# 用户/用户组
cat /etc/passwd | grep mysql
cat /etc/group | grep mysql

# 查看版本
mysqladmin --version

# 启动
systemctl status mysqld
systemctl start mysqld
systemctl enable mysqld

# 管理员初始化（从日志中查找默认密码）
sudo grep 'temporary password' /var/log/mysqld.log

# 修改密码
mysql -uroot -p
alter user 'root'@'localhost' identified by 'passwd';
```

### mysql配置
```mysql
# 默认路径
/var/lib/mysql # 数据库文件存放路径
/usr/share/mysql # 默认配置文件目录
/usr/bin # 相关命令
/etc/init.d/mysql # 启停脚本

# 查询默认字符集（mysql8.0默认utf8mb4）
show variables like '%char%';

# 修改字符配置
[client]
default-character-set=utf8

[mysqld]
port=3306
socket=/var/lib/mysql/mysql.sock
character_set_server=utf8
character_set_client=utf8
collation-server=utf8_general_ci

[mysql]
default-character-set=utf8
```
* log-bin # 二进制日志文件，用于主从复制
* log-err # 记录严重警告、错误、重启信息，默认关系
* log # 查询日志
* /var/lib/mysql # 数据文件
* frm文件 # 存放表结构
* myd文件 # 数据文件
* myi文件 # 索引文件

### mysql逻辑架构
```mysql
Connectors -> Connection pool -> SQL Interface -> Storage Engines-> Files&Logs
                              -> Parser
                              -> Optimizer
                              -> Caches & Buffers
```

### mysql存储引擎
```mysql
show engines;
show variables like '%storage_engine%';
```

| 对比     | MyISAM             | InnoDB             |
| -------- | ------------------ | ------------------ |
| 主外键   | 不支持             | 支持               |
| 事务     | 不支持             | 支持               |
| 行表锁   | 表锁，不适合高并发 | 行锁，适合高并发   |
| 缓存     | 只缓存索引         | 缓存索引和真实数据 |
| 表空间   | 小                 | 大                 |
| 关注点   | 性能               | 事务               |
| 默认安装 | 安装               | 安装               |

* Percona Server 开发 XtraDB存储引擎，性能更好
* MariaDB扩展提供XtraDB存储引擎
* AliSql、AliRedis

## 索引优化分析
### 性能问题，执行时间，等待时间
1. 查询语句问题
2. 索引失效问题（单值索引，复合索引）
3. 关联查询join太多
4. 服务器调优、参数设置、缓存设置

### 常见通用的join查询
* 手写顺序: select->from->join->on->where->group->having->distinct->order->limit
* 执行顺序: from->on->join->where->group->having->select->distinct->order->limit


* inner join # 交集
* left join # 交集 + 左表
* right join # 交集 + 右表
* left join where right is null # 左表
* right join where left is null # 右表
* full outer join # 全连接
* full outer join where right is null or left is null # 全连接去除交集

### 索引简介
* 提高查询效率的数据结构
* 排好序用于快速查找
* 索引本身也很大，以文件形式存储在磁盘上
* 索引优势
    * 索引提高检索效率，降低IO成本
    * 降低排序成本，降低CPU消耗
* 索引劣势
    * 降低更新表效率，insert、update、delete
    * 占用硬盘空间
    * 需要反复优化
* 索引分类
    * 单值索引，只包含单列的索引
    * 唯一索引，列值必须唯一，但允许有空值
    * 复合索引，一个索引包含多个列
* 基本语法
```mysql
# 创建索引
create [unique] index indexName on mytable(columnname(length))
alter mytable add [unique] index [indexName] on (columname(length))

# 查看
show index from tbl_emp

# 删除
drop index [indexName] on talbe;

# 使用alter创建索引的四种方法
## 添加一个主键，索引值必须唯一且不能为null
alter table tbl_name add primary key(column_list);
## 创建索引的值必须唯一，但可以包含null
alter table tbl_name add unique index_name(column_list);
## 添加普通索引，索引值可能重复
alter table tbl_name add index index_name(column_list);
## 全文索引
alter table tbl_name add fulltext index_name(column_list);
```

#### 索引结构
* Btree
* Hash
* full-text
* R-True

#### 哪些情况需要建立索引
1. 主键自动建立索引
2. 频繁查询的字段
3. 外键字段建立索引
4. 频繁更新的字段不适合建立索引
5. where条件里用不到的字段不创建索引
6. 高并发下倾向创建组合索引
7. 查询中的排序字段适合建立索引，提高排序速度
8. 查询中统计或者分组的字段，提高分组速度

#### 哪些情况不要建立索引
1. 表记录太少不建立索引
2. 经常增删改的表不要建立索引
3. 重复、分布平均的字段值不建立索引

### 性能分析
#### MySql Query Optimizer
* Mysql自带查询优化器

#### MySQL常见瓶颈
* cpu瓶颈：数据装入内存或从磁盘上读取数据
* IO瓶颈：装入数据远大于内存容量
* 服务器性能瓶颈

#### Explain
* 模拟执行select过程
* 功能：
    * 表的读取顺序
    * 数据读取操作的操作类型
    * 哪些索引可以使用
    * 哪些索引被实际使用
    * 表之间的引用
    * 每张表有多少行被优化器查询
* 分析
    * 使用方法：`explain select * from table;`
    * `\G`竖版显示
    * id
        * id相同则从上到下执行
        * id不同则值越大优先级越高
        * id有相同有不同，先按优先级，再按顺序执行
    * select_type
        * simple：简单查询，不包含子查询、union
        * primary：包含任何复杂查询，最外层查询为primary
        * subquery：子查询 
        * derived: 衍生表，临时表
        * union: 第二个select出现在union之后，标记为union，union包含在from子句的子查询中，外则select则被标记为derived
        * union result：从union表获取的结果
    * table
    * type，优化到range、ref级别
        * system > const > eq_ref > ref > range > index > all
        * system：只有一行记录，const类型特例
        * const：通过索引一次就找到，比较primary key或者unique能找到唯一记录
        * eq_ref：唯一性索引扫描
        * ref：非唯一性索引扫描，索引访问后返回多条记录，复合索引只使用部分
        * range：between，<, >, in, 开始于索引的某一点，结束与某一点
        * index：全索引扫描，通常比all快，例如select id
        * all: 全表扫描，例如select *
    * possible_keys：所有涉及到的索引
    * key：实际使用的索引，覆盖索引不显示
    * ken_len：索引字段的最大可能长度
    * ref：索引的哪一列被实际使用
    * row：估计读取行数，越少越好
    * extra：其他重要信息
        * using filesort：无法利用索引排序，文件排序
        * using temporary: 产生了临时表
        * using index：select命中索引，避免访问数据行
        * using where：where条件命中索引，避免访问数据行
        * using join buffer
        * impossible where
        * select tables optimized away
        * distinct

### 索引优化
#### 单表
#### 双表
* 左连接右表加索引
* 右连接左表加索引

#### 三表
* 三表同两表

#### 避免索引失效
1. 全值匹配我最爱
2. 最佳左前缀法则：多值索引只能按顺序访问，第一个字段不能少，中间字段不能越过
3. 不在索引列上做任何操作（计算、函数、（自动or手动）类型转换），会导致索引失效而转向全表扫描
4. 存储引擎不能使用索引中范围条件右边的列
5. 尽量使用覆盖索引（只访问索引的查询（索引列和查询列一致）），减少select *
6. mysql在使用不等于的时候无法使用索引会导致全表扫描
7. is null，is not null也无法使用索引（设计时避免空值）
8. like以通配符开头(如'%abc..')mysql索引失效会变成全表扫描
9. 字符串不加单引号索引失效（造成隐式类型转换）
10. 少用or，用它来连接时会索引失效
11. 总结：


## 查询截取分析
1. 慢查询的开启和捕获
2. explain+慢SQL分析
3. show profile查询SQL在Mysql服务器里面的执行细节和生命周期
4. SQL数据库服务器的参数调优

### 查询优化

#### 小表驱动大表（外层循环少）
* A > B: select * from A where id in (select id from B)
* A < B: select * from A where exists (select 1 from B where B.id == A.id)

#### Order by尽量使用index排序，避免使用Filesort
1. Order by多字段排序要按照index顺序，也不能越过字段排序
2. Oder by 字段排序方式相同效率高，字段排序顺序不同会索引失效，影响性能
3. 避免使用select *,节省缓冲区
4. 增大sort_buffer_size
5. 增大max_length_for_sort_data, 避免超长时增加磁盘IO


### 慢查询日志

1. 默认不开启慢查询日志，调优时手动打开
2. 日志位置：`show variables like '%show_query_log%'`
3. 临时开启：`set global show_query_log = 1`
4. 配置开启：
```mysql
[mysqld]
slow_query_log=1
slow_query_log_file=path
log_query_time=3
log_output=FILE
```
5. 慢时间阈值（默认10秒）：`show global variables like 'long_query_time%'`
6. 设置阈值：`set global long_query_time=3; `
7. 模拟慢查询：`select sleep(4)`
8. 慢查询个数：`show global status like '%Slow_queries%'`
9. 批量分析脚本：`mysqldumpslow -s -r -t 10 /var/lib/mysql/log.log`


### 批量插入测试数据


### show profile
* 分析资源消耗
* 测量SQL调优
* 默认关闭，仅储存15条记录
* 使用方法：
    * 需要mysql版本支持
    * 打开`set profiling=on`
    * 查看是否打开`show variables like 'profiling'`
    * 使用：`show prifile`
* 分析
    * `show prifile cpu, block io for query ID`
    * 异常1：converting HEAP to MyISAM，内存不够使用磁盘
    * 异常2：creating tmp table，创建临时表
    * 异常3：copying to tmp table on disk，内存中临时表复制到磁盘
    * 异常4：locked

### 全局查询日志

## mysql锁机制


### 行锁
### 表锁
### 页锁

### 优化
```mysql
show open tables

```

## 主从复制
1. slave会从master读取binlog
* 每个slave只有一个master
* 每个slave只能有一个唯一的服务器ID
* 

### 配置方法
1. mysql版本一致且以服务运行
2. 主从配置在[mysqld]
    * 主：
    * 从：



### 
### 





## 面试题

<https://blog.csdn.net/qq_22222499/article/details/79060495>
<https://blog.csdn.net/ThinkWon/article/details/104778621>
