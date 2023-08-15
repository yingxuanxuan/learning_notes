# Nginx

# 目录
[toc]


# 参考资料

## 文档

### 中文文档
* 此文档是原文档基础之上的翻译
<https://docshome.gitbook.io/nginx-docs/>

### 英文文档
* Nginx另有Plus收费版本，nginx.org为开源版本。
<http://nginx.org/en/docs/>


# 快速入门示例

## 知识点
* Nginx读作engine x
* 是一种HTTP服务器
* 是一种反向代理，支持负载均衡
* 特点为高性能、高并发、低内存消耗
* 单服务器可支撑约5w并发

## 功能概述
* 正向代理：client通过代理服务器访问server
* 反向代理：server端转发客户请求的代理，client无感知
* 负载均衡：反向代理的一种，根据配置将client请求转发到多个server端
* 动静分离：反向代理的一种，根据配置将动态请求、静态请求分别转发到后端，也可以由nginx自身服务静态资源

## 原理分析

## 

## 高可用集群

# 安装
<http://nginx.org/en/linux_packages.html>

## 1、epel安装（推荐）
```
sudo yum search epel
sudo yum install epel-release
sudo yum search nginx
sudo yum install nginx
```

## 2、docker安装

## 3、官方repo仓库安装
* repo：/etc/yum.repos.d/nginx.repo   
```bash
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/mainline/OS/OSRELEASE/$basearch/
gpgcheck=0/1
enabled=1
```
* 参数说明：  
    OS: rhel/centos  
    OSRELEASE: 5/6/7  

* 导入repo签名
``` sh
wget http://nginx.org/keys/nginx_signing.key  
sudo rpm --import nginx_signing.key  
```

## 编译安装
--prefix=/etc/nginx  
--sbin-path=/usr/sbin/nginx  
--conf-path=/etc/nginx/nginx.conf  
--error-log-path=/var/log/nginx/error.log  
--http-log-path=/var/log/nginx/access.log  
--pid-path=/var/run/nginx.pid  
--lock-path=/var/run/nginx.lock  
--http-client-body-temp-path=/var/cache/nginx/client_temp  
--http-proxy-temp-path=/var/cache/nginx/proxy_temp  
--http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp  
--http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp  
--http-scgi-temp-path=/var/cache/nginx/scgi_temp  
--user=nginx  
--group=nginx  


# 控制指令

## 信号
``` sh
nginx -s stop # fast shutdown  
nginx -s quit # graceful shutdown  
nginx -s reload # reloading config  
nginx -s reopen # reopen log  
```

## 命令

* 测试配置
``` sh
sudo nginx -t  
sudo nginx -T 测试并打印配置  
```

* 查看编译选项
``` sh
sudo nginx -V  
```

* 使用新配置
``` sh
sudo nginx -c new.conf  
```

* 动态改变配置
``` sh
sudo nginx -g "pid /var/run/nginx/pid; worker_process `sysctl -n hw.ncpu`;"  
```

## 服务
``` sh
service nginx {start|stop|status|restart|reload|configtest}   
```

# 配置

## 配置概述

### 注释
行内从'#'开始都是注释  

### 指令
指令是一个字符串，可以使用单引号或双引号引起来，一般不用除非指令包含空格  

### 指令参数
使用空格和TAB分开，参数包含复合配置块的使用大括号括起来。  

### 指令上下文
main：业务功能无关参数  
http：http服务相关配置参数  
location：特定URL配置参数  
mail：email相关代理共享配置项  

### 单位
千字节：k,K  
兆字节：m,M  
毫秒：ms  
秒：s  
分钟：m  
小时：h  
日：d  
周：w  
月，30天：M  
年，365天：y  

### 正则表达式
.：匹配除换行符以外的任意字符  
?：重复0次或1次  
+：重复1次或更多次  
*：重复0次或更多次  
\d：匹配数字   
^：开头  
$：末尾  
{n}：重复n次  
{n,}：重复n次或更多次  
[c]：匹配单个字符c  
[a-c]：匹配a-z任意  

()：使用$1, $2引用括号中内容  
\：转译特殊字符  

".(mp4|mov|avi)$"：mp4或mov或avi  

### 常用内置变量
<http://nginx.org/en/docs/http/ngx_http_core_module.html#variables>  
$arg_name：请求 uri 中的 name 参数至  
$args：请求 uri 的所有参数，和 $query_string 相同  
$uri：当前请求的 uri，不带参数  
$request_uri：请求的 uri，带完整参数  
$host：http 请求报文中 host 首部，如果没有 host 首部，则以处理此请求的虚拟主机的主机名替代  
$hostname：nginx 服务运行在主机的主机名  
$remote_addr：客户端 IP  
$remote_port：客户端 port  
$remote_user：使用用户认证时客户端用户输入的用户名  
$request_filename：用户请求中的 URI 经过本地 root  或 alias 转换后映射的本地的文件路径  
$request_method：请求方法  
$server_addr：服务器地址  
$server_name：服务器名称  
$server_port：服务器端口  
$server_protocol：服务器向客户端发送响应时的协议，如 http/1.1，http/1.0  
$scheme：在请求中使用的 scheme，如 https://www.magedu.com/ 中的 https  
$http_name：匹配请求报文中的指定 HEADER，如 $http_host 匹配请求报文中的 host 首部  
$sent_http_name：匹配响应报文中指定的 HEADER，例如 $sent_content_type 匹配响应报文中的 content-type 首部  
$status：响应状态  

### 自定义变量
[set](http://nginx.org/en/docs/http/ngx_http_rewrite_module.html#set)  
[map](http://nginx.org/en/docs/http/ngx_http_map_module.html#map)  
[geo](http://nginx.org/en/docs/http/ngx_http_geo_module.html#geo)  

### 包含配置目录
```
http {
    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*.conf;
    include /etc/nginx/sites-enabled/my_own_conf;
    ...
}
```

配置示例
-----------------------------------------------------------------------------------------
``` nginx
http{
    # http1
    server {
        listen 80;

        # 指定监听IP
        listen 192.168.1.1:80;

        # 指定默认服务器
        listen 80 default_server; 

        # 指定Host
        server_name www.com www.www.com; 

        # 拒绝没有Host的请求
        server_name "";
        return 444;

        # URL相对地址
        location / { 
            root /linux/path; # 静态网页地址
        } 

        # 代理转发
        location /proxy/ { 
            proxy_pass http://localhost:8080;
        }

        # 正则表达式（~开头）
        location ~ \.(gif|jpg|png)$ {
            root /data/images;
        }
    }

    # http2
    server {
        listen 8080;     
    }
}
```

全局配置（core functionality)
-----------------------------------------------------------------------------------------
### user # 用户，用户组配置
语法：user nginx nginx;

### worker_processes # 工作线程数
语法：worker_processes 2  
默认值：与CPU核数相同  
上下文：main  

CPU核数：``` grep ^processor /proc/cpuinfo | wc -l  ```
若开启ssl和gzip应该设置为CPU核数2倍，可以减少I/O操作  

### worker_cpu_affinity # CPU绑定
语法：worker_cpu_affinity 0001 0010 0100 1000  
上下文：main  

### worker_connections # 单工作线程最大连接数
语法：worker_connections number  
默认：worker_connections 512  
上下文：events  

最大连接数是 worker_connections * worker_processes  
反响代理会占用两个连接，最大连接数是 worker_connections * worker_processes / 2  

### worker_rlimit_nofile # 最大文件描述符
语法：worker_rlimit_nofile 10240  
默认值：N/A  
上下文：main  

与ulimit -n最大可能值相同，超过会返回502错误  

### use # 事件模型
语法：use {epoll|kqueue|select|poll|/dev/poll|eventport}  
默认值：Linux下默认为epoll，Unix为kqueue，操作系统不支持时使用select  
上下文：event  

### error_log # 日志存储位置
语法：error_log file [level];  
默认：error_log logs/error.log error;  
上下文：main, http, mail, stream, server, location  

日志级别：  
debug, info, notice, warn, error, crit, alert, or emerg  

debug需要开启--with-debug编译选项  
1.7.11以上可以设置stream日志级别  
1.9.0以上可以设置mail日志级别  

### pid # PID存储位置
语法：pid /path/to/pid;  


HTTP核心配置（ngx_http_core_module）
-----------------------------------------------------------------------------------------
### client_header_buffer_size
语法：client_header_buffer_size size;  
默认：1K  
上下文：http, server  

使用getconf PAGESIZE获取linux系统分页大小，一般设置为系统分页的整数倍。  

### client_body_buffer_size
语法：client_body_buffer_size size;  
默认：32bit 8k | 64bit 16k  
上下文：http, server, location  

### listen
语法：listen address[:port] [default_server] [ssl]   
          listen port [default_server] [ssl]   
默认：listen *:80 | *:8000;  
上下文：server  

不配置listen时，默认使用80端口，若nginx有root权限则使用8000端口  
default_server，一个端口下host不匹配时默认的服务器  
ssl，0.7.14以前的版本，一个Server必须全部是http或https，ssl使一个server可以同时是http和https  

### server_name  
语法：server_name name ...;  
默认：server_name "";  
上下文：server  

server_name example.com www.example.com;  
server_name example.com *.example.com www.example.*;  
server_name .example.com;  
server_name ~^www\d+\.example\.com$;  
server_name _; （使用上一个server_name）  
server_name ""; （接受不存在Host字段的请求）  

### location
语法：location [ = | ~ | ~* | ^~ ] uri { ... }  
            location @name { ... }  
默认：    —  
上下文：server, location  

会解析%xx格式文字
会解析相对路径 . 或 .. 
默认会合并相邻斜杠（merge_slashes on）

如果location请求被proxy_pass、fastcgi_pass、uwsgi_pass、scgi_pass、memcached_pass处理，若如”location /user/“带斜杠，则会正常处理。若如”location /user“不带斜杠，ngxin会发送一个301重定向到"location /user/"。若这并不是所要的，需要分别定义“location /user/”和“location /user”。

URL带斜杠\表示目录，不带斜杠表示文件。不加斜杠访问会尝试访问文件，不存在时返回301重定向到带斜杠\的地址。  
如http://www.example.com/后可以直接加斜杠。但是"http://www.example.com/user"不知道是否加斜杠，可能会造成一次不必要的请求。  

### root
语法：root path;  
默认：root html;  
上下文：http, server, location, if in location  

### alias
语法：alias path  
默认：-  
上下文：location  

可以使用正则表达式：  
``` nginx
location ~ ^/users/(.+\.(?:gif|jpe?g|png))$ {  
    alias /data/w3/images/$1;  
}  
```

root与alias比较：  
使用root指令，location的完整地址会附加到root的地址后面  
使用alias指令，location中不匹配的部分会附加到alias的地址后面  

### client_max_body_size # 大文件上传限制，超出返回HTTP 413
语法：client_max_body_size size;  
默认：client_max_body_size 1m;  
上下文：http, server, location  

### send_file # 直接发送文件
语法：sendfile on | off;  
默认：sendfile off;  
上下文：http, server, location, if in location  
默认nginx会把文件copy到内存后再发送，打开send_file后会直接从文件fd复制到socket fd。  

### tcp_nopush 
语法：tcp_nopush on | off;  
默认：tcp_nopush off;  
上下文：http, server, location  

在一个包中发送响应头和文件  
数据包累积满时才发送  
依赖于sendfile打开   

### keepalive_timeout # 连接超时时间
语法：keepalive_timeout timeout [header_timeout];  
默认：keepalive_timeout 75s;  
上下文：http, server, location  

设置为0即关闭  
header_timeout：设置响应头"Keep-Alive: timeout=time"，可以和服务器端不一致  

### open_file_cache # 文件句柄池
语法：open_file_cache off;  
          open_file_cache max=N [inactive=time];  
默认：open_file_cache off;  
上下文：http, server, location  

### open_file_cache # 句柄有效时长  
语法：open_file_cache_valid time;  
默认：open_file_cache_valid 60s;  
上下文：http, server, location  

### open_file_min_uses # 去激活前最低使用次数
语法：open_file_cache_min_uses number;  
默认：open_file_cache_min_uses 1;  
上下文：http, server, location  

### open_file_cache_error # 是否缓存文件错误
语法：open_file_cache_errors on | off;  
默认：open_file_cache_errors off;  
上下文：http, server, location  

### aio # 异步IO
语法：aio on | off | threads[=pool];  
默认：aio off;  
上下文：http, server, location  


HTTP日志配置（ngx_http_log_module）
-----------------------------------------------------------------------------------------
### access_log
语法：access_log path [format [buffer=size] [gzip[=level]] [flush=time] [if=condition]];  
          access_log off;  
默认：access_log logs/access.log combined;  
上下文：http, server, location, if in location, limit_except  

### log_format
语法：log_format name string ...;  
默认：log_format combined "...";  
上下文：http  

系统变量：  
<http://nginx.org/en/docs/varindex.html>

日志变量：  
$bytes_sent：发送给客户端的字节数  
$connection：连接序号  
$connection_requests：当前连接请求序号  
$msec：日志写入millisecond时间戳  
$pipe：“p” if request was pipelined, “.” otherwise  
$request_length：请求长度 (包含request line, header, and request body)  
$request_time：请求时间，从收到第一个字节到响应最后一个字节的时间段   
$status：响应状态  
$time_iso8601：ISO 8601格式的本地实际  
$time_local：本地时间  

预定义combined日志格式：  
```
log_format combined '$remote_addr - $remote_user [$time_local] '  
                    '"$request" $status $body_bytes_sent '  
                    '"$http_referer" "$http_user_agent"';  
```

### open_log_file
语法：open_log_file_cache max=N [inactive=time] [min_uses=N] [valid=time];  
          open_log_file_cache off;  
默认：open_log_file_cache off;  
上下文：http, server, location  


HTTP字符集配置（ngx_http_charset_module）
-----------------------------------------------------------------------------------------
### charset
语法：charset charset | off;  
默认：charset off;  
上下文：http, server, location, if in location  

### source_charset
语法：source_charset charset;  
默认：—  
上下文：http, server, location, if in location  


HTTP gzip配置（ngx_http_gzip_module）
-----------------------------------------------------------------------------------------
### gzip # gzip开关
语法：gzip on | off;  
默认：gzip off;  
上下文：http, server, location, if in location  

### gzip_buffers number # 缓冲区大小
语法：gzip_buffers number size;  
默认：gzip_buffers 32 4k|16 8k;  
上下文：http, server, location  

### gzip_comp_level # 压缩级别
语法：gzip_comp_level level;  
默认：gzip_comp_level 1;  
上下文：http, server, location  

压缩级别：1-9  

### gzip_disable User-Agent过滤
语法：gzip_disable regex ...;  
默认：—  
上下文：http, server, location  

例如：```gzip_disable "MSIE [4-6]\."  ```

### gzip_min_length 压缩最小长度  
语法：gzip_min_length length;  
默认：gzip_min_length 20;  
上下文：http, server, location  

### gzip_types 压缩mime类型
语法：gzip_types mime-type ...;  
默认：gzip_types text/html;  
上下文：http, server, location  

text/html总是会被压缩  
使用"*"压缩所有类型   


HTTP 重定向配置（ngx_http_rewrite_module）
-----------------------------------------------------------------------------------------
<http://nginx.org/en/docs/http/ngx_http_rewrite_module.html>


Web Server
====================================================
Server name
-----------------------------------------------------------------------------------------
### 匹配顺序
1、Exact name  
2、Longest wildcard starting with an asterisk, such as *.example.org  
3、Longest wildcard ending with an asterisk, such as mail.*  
4、First matching regular expression (in order of appearance in the configuration file)  
5、default_server  

Location
-----------------------------------------------------------------------------------------
### 普通匹配
location /  
location /image  
按最长匹配规则，所有URI都会匹配（location /）规则，正则表达式和比（location /）长的匹配（如 location /image）会优先匹配  

### 精确匹配（=）
location = /  
location = /test  
最长匹配，匹配后不再检查其他规则  

### 正则匹配（~）  
tilde（~）：默认正则大小写敏感匹配  
tilde-asterisk（~*）：忽略大小写  

忽略大小写匹配.html 和.htm  
```  
location ~* \.html?  
```

### 停止正则匹配（^~）
在路径中找到最长匹配后，不再检查正则表达式  

### @locationname内部redirect匹配
location @locationname  

``` nginx
error_page 404 = @fetch;

location @fetch{
  proxy_pass http://fetch;
}
```

### 匹配顺序
1、Test the URI against all prefix strings (pathnames).  
      使用（against）全部路径检查URI  
2、The = (equals sign) modifier defines an exact match of the URI and a prefix string (pathnames).   
      If the exact match is found, the search stops.  
      检查是否有精确匹配（=），如果精确匹配被找到则停止搜索  
3、If the ^~ (caret-tilde) modifier prepends the longest matching prefix string (pathnames), the regular expressions are not checked.  
      检查最长匹配中，如果有最短匹配（^~）符号，则忽略正则表达式（已经是最长匹配）  
4、Store the longest matching prefix string (pathnames).  
      存储最长匹配的路径  
5、Test the URI against regular expressions.  
      使用（against）全部正则表达式检查URI  
6、Break on the first matching regular expression and use the corresponding location.  
      正则表达式中按配置顺序匹配  
7、If no regular expression matches, use the location corresponding to the stored prefix string (pathnames).  
      没有正则表达式匹配则使用前面存储的路径  


### 性能优化
一般首页最常用，配置为：  
``` nginx
location = / {
    proxy_pass http://example.com/index.html
}
```

静态文件提高效率，使用最短匹配，不再检查正则表达式规则：  
``` nginx
location ^~ /static/ {
    root /static/;
}
```
或  
``` nginx
location ~* \.(gif|jpg|jpeg|png|css|js|ico)$ {
    root /res/;
}
```

所有未被匹配的内容，使用通用匹配转发至后端：  
``` nginx
location / {
    proxy_pass http://example.com/index.html
}
```

返回状态码
-----------------------------------------------------------------------------------------
### 无参数状态码  
``` nginx
location /wrong/url {
    return 404;
}
```

### 带参数状态吗
``` nginx
location /permanently/moved/url {
    return 301 http://www.example.com/moved/here;
}
```

重写URL（rewrite）
-----------------------------------------------------------------------------------------
### rewrite
语法：rewrite regular_expression new_uri [break|last|redirect|permanent]  
上下文：server, location, if  

uri重写后会重新选择location  
rewrite值对域名之后的，除去查询参数的部分起作用，例如http://a.com/a/we/index.php?id=1&u=str，只对/a/we/index.php起作用  

### last
停止当前server或location中的rewrite，但是rewrite后的uri会继续查找location，location可能还会执行rewrite。  

### break
停止当前server或location中的rewrite，rewrite后的uri不会继续查找location。  

### redirect | permanent
redirect 302临时重定向  
permanent 301永久重定向  

### server中rewrite会按顺序执行一次  

### location中rewrite在选中location时会执行  


重写HTTP
-----------------------------------------------------------------------------------------
### 更改http请求refer
``` nginx
location / {
    sub_filter /blog/ /blog-staging/;
    sub_filter_once off;
}
```

### 更改host name
``` nginx
location / {
    sub_filter 'href="http://127.0.0.1:8080/' 'href="http://$host/';
    sub_filter 'img src="http://127.0.0.1:8080/' 'img src="http://$host/';
    sub_filter_once on;
}
```

错误处理
-----------------------------------------------------------------------------------------
### 定义错误页
``` nginx
error_page 404 /404.html
```

### 替换错误码
``` nginx
location /old/path.html {
    error_page 404 =301 http://example.com/new/path.html;
}
```

### 将错误转交处理
``` nginx
server {
    location /images/ {
        error_page 404 = /fetch$uri;
    }

    location /fetch/ {
        proxy_pass http://backend/;
    }
}
```

反向代理 Reverse Proxy
====================================================

转发请求
-----------------------------------------------------------------------------------------
### 转发到网址
``` nginx
proxy_pass http://www.example.com
```

### 转发到IP端口
``` nginx
proxy_pass http://127.0.0.1:8000
```

### 替换location
``` nginx
location /some/path/ {
    proxy_pass http://www.example.com/link/;
}
```
假设访问/some/path/page.html  
会替换成/link/page.html  

### FastCGI
fastcgi_pass

### uwsgi
uwsgi_pass

### SCGI
scgi_pass

### memcached
memcached_pass

### named group（即负载均衡）
<https://www.nginx.com/resources/admin-guide/load-balancer/>

修改转发请求头部
-----------------------------------------------------------------------------------------
默认会删除内容为空的header fields，设置"Host"为$proxy_host，"Connection"为"close"。

### proxy_set_header
语法：proxy_set_header field value;  
默认：proxy_set_header Host $proxy_host;  
          proxy_set_header Connection close;  
上下文：http, server, location  

### 设置Host为原始Host
proxy_set_header Host $host;  

### 设置真实IP为原始IP
proxy_set_header X-Real-IP $remote_addr;  

### 阻止某个字段传给代理（设置为空）
proxy_set_header Accept-Encoding "";   


配置缓存
-----------------------------------------------------------------------------------------
### proxy_buffering（开关）
语法：proxy_buffering on | off;  
默认：proxy_buffering on;  
上下文：http, server, location  

### proxy_buffers（单连接缓存大小块数）
语法：proxy_buffers number size;  
默认：proxy_buffers 8 4k|8k;  
上下文：http, server, location  

### proxy_buffer_size（响应读取阈值）
语法：proxy_buffer_size size;  
默认：proxy_buffer_size 4k|8k;  
上下文：http, server, location  

### 低延迟交互应用时关闭proxy_buffer（同步处理）
proxy_buffering off;  


绑定出口IP
-----------------------------------------------------------------------------------------
### 绑定IP
proxy_bind 127.0.0.1;  

### 绑定变量
proxy_bind $server_addr;  


内容缓存 Content Caching
====================================================
<https://www.nginx.com/resources/admin-guide/content-caching/>

开启响应缓存
-----------------------------------------------------------------------------------------
### proxy_cache 开启或关闭代理缓存
语法：proxy_cache zone | off;  
默认：proxy_cache off;  
上下文：http, server, location  

### proxy_cache_path 定义缓存区
语法：proxy_cache_path path [levels=levels] [use_temp_path=on|off] keys_zone=name:size [inactive=time] [max_size=size] [loader_files=number] [loader_sleep=time] [loader_threshold=time] [purger=on|off] [purger_files=number] [purger_sleep=time] [purger_threshold=time];  
默认：N/A 
上下文：http  

### keys_zone # 定义缓存区名称，大小，超时时间，
proxy_cache_path /path/to/cache keys_zone=one:10m  

这个大小是key存储空间大小，不是数据存储空间大小，1m约能存储8k个key。  

### levels # 指定缓存子目录数
假设设置为levels=1:2   
缓存文件key为b7f54b2df7773722d382f4809d65029c  
nginx会取key最后1个字符c为一级目录，再取倒数2、3个字符29为二级目录  
最多设置三级目录 levels=2:2:2  

### inactive
不访问老化时间  

### use_temp_path
默认会先把文件写入临时目录（proxy_temp_path），若临时目录与缓存目录不在同一个文件系统，会造成两次文件写入  
建议设置为off  


设置缓存加载
-----------------------------------------------------------------------------------------
一次加载过多会影响启动速度和性能  

### loader_threshold
遍历时间阈值，默认200millliseconds  

### loader_files
遍历文件阈值，默认100  

### loader_sleeps
遍历间隔事件，默认50毫秒  

### 示例
``` nginx
proxy_cache_path /path/to/cache keys_zone=one:10m loader_threshold=300 loader_files=200
```

缓存设置
-----------------------------------------------------------------------------------------
### 设置缓存key计算方法
proxy_cache_key "$host$request_uri$cookie_user";  

### 设置缓存条件：最小请求次数（只有被最常访问的数据才被缓存）
proxy_cache_min_users 5;  

### 设置缓存methods
proxy_cache_methods GET HEAD POST  
默认只缓存GET和HEAD方法  

### 缓存更新锁
语法：proxy_cache_lock on|off;  
默认：proxy_cache_lock off;  
上下文：http, server, location  
当多个客户端同时请求一个缓存中不存在的文件时，只有当请求中第一个请求得到正确缓存数据后，其他请求才得到相应。  
默认关闭，所有客户端都会与后端通信。  

### 使用缓存替代错误相应
proxy_cache_use_stale error timeout http_500 http_502 http_503 http_504;  

### 缓存正在更新时，先发送旧内容，触发更新的请求会等待服务器返回新结果
proxy_cache_use_stale updating  


限制或绕过缓存
-----------------------------------------------------------------------------------------
默认缓存永久缓存，超过限制按最近最少访问老化  

### 设置缓存过期时间
proxy_cache_valid any           5m;  

### 按状态码设置缓存过期时间
proxy_cache_valid 200 302 10m;  
proxy_cache_valid 404          1m;  

### 如果有string参数为空 或 值参数不等于0 则不缓存
proxy_cache_bypass $cookie_nocache $arg_nocache$arg_comment;  

### 如果有string不为空 或 值不等于0 则不缓存
proxy_no_cache $cookie_nocache $arg_nocache$arg_comment;  


缓存手动老化 PURGE
-----------------------------------------------------------------------------------------


缓存分段
-----------------------------------------------------------------------------------------


综合配置示例
-----------------------------------------------------------------------------------------


静态文件
====================================================
<https://www.nginx.com/resources/admin-guide/serving-static-content/>


root目录和Index文件
-----------------------------------------------------------------------------------------
### root使用示例
``` nginx
server {
    root /www/data;

    location / {
        # 路径为root的路径：/www/data
    }

    location /images/ {
        # 路径为root的路径+目录的路径：/www/data/image/
    }

    location ~ \.(mp3|mp4) {
        # 路径被覆盖为：/www/media
        root /www/media;
    }
}
```

### 以斜杠"\"结尾的location
以斜杠"\"结尾的location会被认为是目录，nginx会试图在其中找到index.html文件  
如：假设URI为/images/some/path/，nginx会寻找/www/data/images/some/path/index.html  

### autoindex 自动创建index.html
语法：autoindex on | off;  
默认：autoindex off;  
上下文:http, server, location  

### index 定义index文件名称
语法：index index_file;  


try_file
-----------------------------------------------------------------------------------------
### 不存在则重定向到默认
``` nginx
location /images/ {
    try_files $uri /images/default.gif;
}
```

### 尝试一系列形式，不存在则404
``` nginx
location / {
    try_files $uri $uri/ $uri.html =404;
}
```

### 找不到文件则转到proxy
``` nginx
location / {
    try_files $uri $uri/ @backend;
}
```
``` nginx
location @backend {
    proxy_pass http://backend.example.com;
}
```


性能优化
-----------------------------------------------------------------------------------------
### send_file 直接发送文件
语法：sendfile on | off;  
默认：sendfile off;  
上下文：http, server, location, if in location  
默认nginx会把文件copy到内存后再发送，打开send_file后会直接从文件fd复制到socket fd。  

### sendfile_max_chunk 最大一次发送数据量
语法：sendfile_max_chunk size;  
默认：sendfile_max_chunk 0;  
上下文：http, server, location  
为防止nginx被特大文件占用，需要限制每次最大发送分段为1m  

### tcp_nopush 
语法：tcp_nopush on | off;  
默认：tcp_nopush off;  
上下文：http, server, location  

在一个包中发送响应头和文件  
数据包累积满时才发送  
依赖于sendfile打开  

### tcp_nodelay
语法：tcp_nodelay on | off;  
默认：tcp_nodelay on;  
上下文：http, server, location  

### 增加tcp监听队列长度
查看队列长度方法：  
``` sh
netstat -Lan   
```

临时修改操作系统参数：  
``` sh
sudo sysctl -w net.core.somaxconn=4096
```

永久修改操作系统参数：  
为文件/etc/sysctl.conf添加  
```
net.core.somaxconn = 4096
```

修改nginx配置，一般应大于512：  
``` nginx
listen 80 backlog 4096;
```


正向代理配置示例
====================================================
``` nginx
server{  
        # 不能设置hostname

        # 设置dns及超时时间
        resolver 8.8.8.8;  
        resolver_timeout 30s;   

        # 监听端口
        listen 82;  
        location / {  
                # 代理参数
                proxy_pass http://$http_host$request_uri;  
                proxy_set_header Host $http_host;  

                # 配置缓存大小
                proxy_buffers 256 4k;  

                # 关闭磁盘缓存
                proxy_max_temp_file_size 0;  

                # 配置代理服务器http状态缓存时间
                proxy_connect_timeout 30;  
                proxy_cache_valid 200 302 10m;  
                proxy_cache_valid 301 1h;  
                proxy_cache_valid any 1m;  
        }  
}
```

反向代理配置示例
====================================================
### 例1：
``` nginx
location / {
    proxy_pass      http://192.168.18.201:8080;
    proxy_set_header  X-Real-IP  $remote_addr; # 设置代理转发保留用户ip
}
```

### 例2：
``` nginx
server {  
    listen    80;  
    server_name www.test.com;  
    location / {  
        proxy_pass http://8.8.8.8:80;  
    }  
}  
```

### 例3：
``` nginx
http {  

    # 负载均衡地址池
    upstream http_server_pool {  
        server 192.168.1.2:8080 weight=2 max_fails=2 fail_timeout=30s;  
        server 192.168.1.3:8080 weight=3 max_fails=2 fail_timeout=30s;  
        server 192.168.1.4:8080 weight=4 max_fails=2 fail_timeout=30s;  
    }

    # 一个虚拟主机，用来反向代理http_server_pool这组服务器  
    server {  
        listen       80;  
        # 外网访问的域名          
        server_name  www.test.com;   
        location / {  
            # 后端服务器返回500 503 404错误，自动请求转发到upstream池中另一台服务器  
            proxy_next_upstream error timeout invalid_header http_500 http_503 http_404;  
            proxy_pass http://http_server_pool;  
            proxy_set_header Host www.test.com;  
            proxy_set_header X-Real-IP $remote_addr;  
            proxy_set_header X-Forwarded-For  $proxy_add_x_forwarded_for;  
        }  
        access_log  logs/www.test.com.access.log  combined;  
    }
}
```


ssl
====================================================
<http://nginx.org/en/docs/http/configuring_https_servers.html>

### ssl_protocols SSL协议
语法：ssl_protocols [protocol]  
默认：TLSv1 TLSv1.1 TLSv1.2 
默认值即可  

### ssl_ciphers SSL加密
语法：ssl_ciphers []  
默认：ssl_ciphers HIGH:!aNULL:!MD5  
默认值即可  

### ssl_session_cache SSL会话Cache性能优化
语法：ssl_session_cache shared:SSL:10m;  
默认：  
1m Cache约4000个会话。  

### ssl_session_time SSL会话超时时间
语法：ssl_session_time [time]  
默认：5分钟  
可以增加会话时间以优化。  

### 为缺少中间证书的浏览器添加crt中间证书
``` sh
cat www.example.com.crt bundle.crt > www.example.com.chained.crt  
```
需要保证顺序正确，自己证书在最前  

### 配置示例
``` nginx
server {
    listen     443 ssl;
    ssl         on;

    ssl_certificate      @SSL_CERT@; # 证书，公开
    ssl_certificate_key  @SSL_KEY@; # 密钥，不公开

    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout  10m;

    ssl_prefer_server_ciphers   on;
}
```

### 消息转发
所有http转发到https：  
``` nginx
server {
    listen  80;
    server_name yingxuanxuan.com;

    rewrite ^(.*)$  https://$host$1 permanent;
}
···
   
nginx http错误497转发至https：  
``` nginx
error_page 497  https://$host$uri?$args;    
```

非登陆页面使用http：  
``` nginx
if ($uri !~* "/logging.php$")
{
    rewrite ^/(.*)$ http://$host/$1 redirect;
}
```

登录页面使用https：  
``` nginx
if ($uri ~* "/logging.php$")
{
    rewrite ^/(.*)$ https://$host/$1 redirect;
}
```

### 生成方法
``` sh
openssl req -newkey rsa:2048 -keyout yourname.key -out yourname.csr   
```
CN/Shannxi/Xian/Yingxuanxuan Ltd/Yingxuanxuan/*.yingxuanxuan.com/yingxuanxuan@hotmail.com   
extra部分直接回车   

### 分解生成方法
生成RSA密钥：  
``` sh
openssl genrsa -des3-out 33iq.key 1024
```
复制一个无密码密钥：  
``` sh
openssl rsa -in 33iq.key -out 33iq_nopass.key
```
生成一个证书请求：  
``` sh
openssl req -new-key 33iq.key -out 33iq.csr
```
自己签发证书（不安全连接）：  
``` sh
openssl x509 -req-days365-in 33iq.csr -signkey 33iq.key -out 33iq.crt
```

负载均衡
====================================================
<http://nginx.org/en/docs/http/load_balancing.html>
<https://www.nginx.com/resources/admin-guide/load-balancer/>

### DNS负载均衡
将相同域名配置到不同IP即可

重定向
====================================================
<http://nginx.org/en/docs/http/converting_rewrite_rules.html>


websocket代理
====================================================
<http://nginx.org/en/docs/http/websocket.html>


nginx httl hls module
====================================================
<http://nginx.org/en/docs/http/ngx_http_hls_module.html>
