---
title: Nginx
comments: true
date: 2019-01-26 23:13:19
categories: CS
tags: ['CS', 'Nginx']
---


## nginx 
Nginx是一款轻量级的HTTP服务器，采用事件驱动的异步非阻塞处理方式框架，这让其具有极好的IO性能，时常用于服务端的反向代理和负载均衡。

### 优点
* 支持海量高并发：采用IO多路复用epoll。官方测试Nginx能够支持5万并发链接，实际生产环境中可以支撑2-4万并发连接数。
* 内存消耗少：在主流的服务器中Nginx目前是内存消耗最小的了，比如我们用Nginx+PHP，在3万并发链接下，开启10个Nginx进程消耗150M内存。
* 免费使用可以商业化：Nginx为开源软件，采用的是2-clause BSD-like协议，可以免费使用，并且可以用于商业。
* 配置文件简单：网络和程序配置通俗易懂，即使非专业运维也能看懂。
* 反向代理，负载均衡。正向代理代理的对象是客户端，反向代理代理的对象是服务端。
* 此外，web缓存加速，清除指定url缓存，健康检查，后端服务器故障转移， rewrite重写， 易用。


### 组成
> Nginx由内核和模块组成，其中，内核的设计非常微小和简洁，完成的工作也非常简单，仅仅通过查找配置文件将客户端请求映射到一个location block（location是Nginx配置中的一个指令，用于URL匹配），而在这个location中所配置的每个指令将会启动不同的模块去完成相应的工作。

>Nginx的模块从结构上分为核心模块、基础模块和第三方模块：
* 核心模块：HTTP模块、EVENT模块和MAIL模块
* 基础模块：HTTP Access模块、HTTP FastCGI模块、HTTP Proxy模块和HTTP Rewrite模块，
* 第三方模块：HTTP Upstream Request Hash模块、Notice模块和HTTP Access Key模块。

> Nginx的模块从功能上分为如下三类。
* Handlers（处理器模块）。此类模块直接处理请求，并进行输出内容和修改headers信息等操作。Handlers处理器模块一般只能有一个。
* Filters （过滤器模块）。此类模块主要对其他处理器模块输出的内容进行修改操作，最后由Nginx输出。
* Proxies （代理类模块）。此类模块是Nginx的HTTP Upstream之类的模块，这些模块主要与后端一些服务比如FastCGI等进行交互，实现服务代理和负载均衡等功能。

### 常用命令
/usr/local/nginx/html 默认目录
* nginx -v 查看版本
* nginx- V 查看完整配置信息
* ps -ef | grep nginx 查看nginx进程
* nginx 启动
* nginx -s reload 重新加载配置文件
* nginx -s quit  优雅关闭, 会在处理完当前正在的请求后退出
* nginx -s stop 直接关闭 nginx
* pkill  -9 nginx 强制停止nginx 
* nginx -t 检查修改

### location配置
* 指令格式为：location [ = | ~ | ~* | ^~ ] uri {...}
* “=”  严格匹配
* “~”  区分大小写
* “~*” 不区分大小写
* “^*” 匹配度最高的location匹配

### 其他命令
* netstat -tnl # 查看网络状态tcp number listen
* tail -fn 10 filename # 查看最后10行内容 （cat m1 m2 > file 将文件ml和m2合并后放入文件file中, less, more每次显示一屏, head, tail开头/结尾若干行， touch创建)
* :%s/gamma/xxx/g  # vim 中将所有gamma替换为xxx
* grep -v "#" nginx.conf | grep -v "^$" >> nginx1.conf 把注释追加到nginx1.conf
* du -ah filename # 查看目录及子目录文件大小  （s 不包含子目录文件，h 人类可读）
* wc -l  filename # 统计文件行数Word Count  （l行数， c字节数， m字符数， w字数, L最长行长度）
* awk '{print $1, $2}' # 文本分析处理, 每行按空格或TAB分割，输出文本中的1、2项


### nginx的upstream目前支持4种方式的分配：
* 1、轮询（默认），每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。weight指定轮询几率，weight和访问比率成正比
* 2、ip_hash，每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。
* 3、fair（第三方），按后端服务器的响应时间来分配请求，响应时间短的优先分配。
* 4、url_hash（第三方），按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，后端服务器为缓存时比较有效。


### 配置文件结构
events 和 http 的指令是放在主上下文中，server 放在 http 中, location 放在 server 中。


```
Nginx的配置文件nginx.conf配置详解如下：
 
user nginx nginx ;
# Nginx用户及组：用户 组。
 
worker_processes 8;
# 工作进程：数目。根据硬件调整，通常等于CPU数量或者2倍于CPU。
worker_cpu_affinity 00000001 00000010 00000100 00001000 00010000 00100000 01000000 10000000;
 
error_log  logs/error.log;  
error_log  logs/error.log  warn;  
# 错误日志：存放路径.  参数[ debug | info | notice | warn | error | crit ] 。
 
pid logs/nginx.pid;
# pid（进程标识符）：存放路径。
 
worker_rlimit_nofile 204800;
# 指定进程可以打开的最大描述符：数目。
 
events {
  use epoll;
  # 使用epoll的I/O 事件模型。nginx针对不同的操作系统，有不同的事件模型
  
  worker_connections 204800;
  # 每个工作进程的最大连接数量。尽量大，理论上每台nginx服务器的最大连接数为。worker_processes*worker_connections

  keepalive_timeout 60;
  # keepalive超时时间。
  
  open_file_cache max=65535 inactive=60s;
  # 这个将为打开文件指定缓存，默认是没有启用的，max指定缓存数量，建议和打开文件数一致，inactive是指经过多长时间文件没被请求后删除缓存。
  
  open_file_cache_valid 80s;
  # 这个是指多长时间检查一次缓存的有效信息。
  
  open_file_cache_min_uses 1;
  # open_file_cache指令中的inactive参数时间内文件的最少使用次数，如果超过这个数字，文件描述符一直是在缓存中打开的，如果有一个文件在inactive时间内一次没被使用，它将被移除。

}
 
 
#设定http服务器，利用它的反向代理功能提供负载均衡支持
http {
  include mime.types;
  # 文件扩展名与文件类型映射表
  
  default_type application/octet-stream;
  # 默认文件类型
  
  log_format main '$remote_addr - $remote_user [$time_local] "$request" '
  '$status $body_bytes_sent "$http_referer" '
  '"$http_user_agent" "$http_x_forwarded_for"';
  log_format log404 '$status [$time_local] $remote_addr $host$request_uri $sent_http_location';
  # 日志格式设置。
  # $remote_addr与$http_x_forwarded_for 反向代理服务器的iP地址；原有客户端的IP地址和原来客户端的请求的服务器地址
  # $remote_user：用来记录客户端用户名称；
  # $time_local： 用来记录访问时间与时区；
  # $request： 用来记录请求的url与http协议；
  # $status： 用来记录请求状态；成功是200，
  # $body_bytes_sent ：记录发送给客户端文件主体内容大小；
  # $http_referer：用来记录从那个页面链接访问过来的；
  # $http_user_agent：记录客户浏览器的相关信息；

  # Load modular configuration files from the /etc/nginx/conf.d directory.
  include /etc/nginx/conf.d/*.conf;
  map $http_upgrade $connection_upgrade {
      default upgrade;
      '' close;
  }
  
  access_log  logs/host.access.log  main;
  access_log  logs/host.access.404.log  log404;
  # 用了log_format指令设置了日志格式之后，需要用access_log指令指定日志文件的存放路径；
  
  server_names_hash_bucket_size 128;
  # 服务器名字的hash表大小
  
  client_header_buffer_size 4k;
  # 客户端请求头部的缓冲区大小。这个可以根据你的系统分页大小来设置，一般系统分页都要大于1k。 分页大小可以用命令getconf PAGESIZE 取得。

  client_max_body_size 5m;  
  # 允许客户端请求最大字节数

  client_body_buffer_size 128k; 
  # 缓冲区代理缓存用户端请求最大字节数
  
  large_client_header_buffers 8 128k;
  # 客户请求头缓冲大小。nginx默认会用client_header_buffer_size这个buffer来读取header值，如果header过大，它会使用large_client_header_buffers来读取。
  
  open_file_cache max=102400 inactive=20s;
  # 指定缓存是否启用。
  open_file_cache_valid 30s; 
  open_file_cache_min_uses 2; 
  open_file_cache_errors on;
  

  autoindex on;
  # 开启目录列表访问，合适下载服务器，默认关闭
  
  sendfile on;
  # 是否调用sendfile函数（zero copy方式）来输出文件，对于普通应用，必须设为on。如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络IO处理速度，降低系统uptime。
  
  tcp_nopush on;
  # 防止网路阻塞

  tcp_nodelay on;
  # 防止网络阻塞

  keepalive_timeout 120; 
  # 长连接超时时间，单位是秒  

  fastcgi_connect_timeout 300;
  fastcgi_send_timeout 300;
  fastcgi_read_timeout 300;
  fastcgi_buffer_size 64k;
  fastcgi_buffers 4 64k;
  fastcgi_busy_buffers_size 128k;
  fastcgi_temp_file_write_size 128k;
  # FastCGI相关参数是为了改善网站的性能：减少资源占用，提高访问速度


  gzip on; 
  # 开启gzip压缩输出
  gzip_min_length 1k; 
  # 最小压缩文件大小
  gzip_buffers 4 16k; 
  # 压缩缓冲区
  gzip_http_version 1.0;
  # 压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
  gzip_comp_level 2; 
  # 压缩等级
  gzip_types text/plain application/x-javascript text/css application/xml;
  # 压缩类型，默认就已经包含text/html
  gzip_vary on;


  proxy_connect_timeout 90; 
  # 代理超时时间, nginx和后端服务器连接的超时时间, 发起握手等候响应超时时间
  proxy_read_timeout 180;
  # 代理接收超时，连接成功后等候后端服务器响应时间, 也可以说是后端服务器处理请求的时间
  proxy_send_timeout 180;
  # 代理发送超时，后端服务器数据回传时间，就是在规定时间之内后端服务器必须传完所有的数据
  proxy_buffer_size 256k;
  # 保存用户头信息的缓冲区大小
  proxy_buffers 4 256k;
  # 设置用于读取应答（来自被代理服务器）的缓冲区数目和大小，默认情况也为分页大小，根据操作系统的不同可能是4k或者8k
  proxy_temp_file_write_size 256k;
  # 设置在写入proxy_temp_path时数据的大小，预防一个工作进程在传递文件时阻塞太长
  proxy_temp_path /data0/proxy_temp_dir;
  # proxy_temp_path和proxy_cache_path指定的路径必须在同一分区
  proxy_cache_path /data0/proxy_cache_dir levels=1:2 keys_zone=cache_one:200m inactive=1d max_size=30g;
  #设置内存缓存空间大小为200MB，1天没有被访问的内容自动清除，硬盘缓存空间大小为30GB。
  proxy_intercept_errors on;
  # 表示使nginx阻止HTTP应答代码为400或者更高的应答


  upstream proxy_svr {
    server 127.0.0.1:8000 weight=1  max_fails=2 fail_timeout=30s;  
    server 127.0.0.1:8001 weight=1  max_fails=2 fail_timeout=30s;  
  }


  # 默认配置
  server {
    listen       80;
    server_name  localhost;
    charset koi8-r;
    access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
        index index.html
    }

  }

  # 定义某个负载均衡服务器   
  server {
    listen   4545;  
    server_name  127.0.0.1;  
    keepalive_requests 120; # 单连接请求上限次数。  
    location  ~*^.+$ {       # 请求的url过滤，正则匹配，~为区分大小写，~*为不区分大小写。
      root path;  #根目录
      index index.html;  # 设置默认页
      proxy_pass  http://proxy_svr/movie;  # 请求转向mysvr 定义的服务器列表
      deny 127.0.0.2;  # 拒绝的ip
      allow 172.0.0.3; # 允许的ip           
    } 
  }

  
  # 设定查看Nginx状态的地址
  location /NginxStatus {
    stub_status on;
    access_log on;
    auth_basic "NginxStatus";
    auth_basic_user_file conf/htpasswd;
  }

  # HTTPS配置
  server {  
    listen 443 ssl;  #ssl端口  
    server_name  test.com;  
    #指定PEM格式的证书文件   
    ssl_certificate      /etc/nginx/test.pem;   
    #指定PEM格式的私钥文件  
    ssl_certificate_key  /etc/nginx/test.key;  

  }


}


```