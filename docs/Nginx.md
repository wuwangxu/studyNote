# Nginx学习

Nginx是一个开源且高性能、可靠的HTTP中间件、代理服务。

## Nginx的版本
[Nginx官网](http://nginx.org/)

1. Mainline version 开发版本
2. Stable version 稳定版本
3. Legacy version 历史版本

## 初始化网络环境
```
iptables -L   
iptables -F

iptables -t nat -L
iptables -t nat -F

getenforce
setenforce 0

```

## 安装目录

命令：
  ```bash
  rpm -ql nginx
  ```

## Nginx 常用命令
```bash
nginx -t -c /etc/nginx/nginx.conf #测试语法是否正确
systemctl reload nginx #重启nginx
nginx -s reload -c /etc/nginx/nginx.conf

```

## Nginx日志类型

- error.log
- access_log

log_format 规定log的格式

### Nginx变量

* 内置变量
* HTTP请求变量
* 自定义变量
  ```Shell
  $remote_addr #nginx客户端地址
  $remote_user #nginx客户端请求的用户名
  $time_local #请求的时间
  $request #请求信息
  $http_referer #上级地址
  $http_user_agent #请求的头信息
  ```
  *

## Nginx模块
  * Nginx官方模块
  * 第三方模块
    ```bash
    --with-http_stub_status_module #状态  stub_status;
    --with-http_sub_module #http内容替换
    --with-compat 
    --with-file-aio 
    --with-threads 
    --with-http_v2_module 
    --with-http_addition_module 
    --with-http_auth_request_module 
    --with-http_dav_module 
    --with-http_flv_module 
    --with-http_gunzip_module 
    --with-http_gzip_static_module 
    --with-http_mp4_module 
    --with-http_random_index_module 
    --with-http_realip_module 
    --with-http_secure_link_module 
    --with-http_slice_module 
    --with-http_ssl_module 
    --with-mail 
    --with-mail_ssl_module 
    --with-stream 
    --with-stream_realip_module 
    --with-stream_ssl_module 
    --with-stream_ssl_preread_module
    --with-cc-opt
    ```

## Nginx的访问控制
  * 基于IP的访问控制 - http_access_module
  * 基于用户的信任登录 - http_auth_basic_module

## Nginx常见问题
  1. 相同server_name多个虚拟主机优先级访问  
    
      文件名顺序

  2. location匹配优先级
  - =     进行普通字符精确匹配
  - ^~    前缀匹配
  - ~\~*  正则匹配

  3. Nginx的try_files的使用  
     按顺序检查文件是否存在
     ```bash
     location /tryFiles {
      root /opt/app/code/cache;
      try_files $uri @java_page; #先走root下的$uri，文件不存在，去@java_page
     }
     location @java_page {
      proxy_pass http://127.0.0.1:9090;
     }

     ```

  4. Nginx的alias和root区别
  http://www.wxsit.cn/request_path/image/cat.png
  * root 方式
    ```bash
    location /request_path/image/ {
      root /local_path/image/;
    }
    ```
    /local_path/image/**request_path/image**/cat.png
  * alias 方式
    ```bash
    location /request_path/image/ {
      root /local_path/image/;
    }
    ```
    /local_path/image/cat.png  

  5. 用什么方式传递用户的真实IP地址  
    设置一个约定的自定义头

  6.其他  
  
    1. Nginx：413 Request Entity Too Large  
    用户上传文件限制 client_max_body_size  
    2. 502 Bad Gateway  
    后端服务无响应  
    3. 504 Gateway Time-out  
    后端服务执行超时

## Nginx性能优化

  ### 性能优化考虑点
  * 当前系统结构瓶颈  
      观察指标、压力测试，stub_status
  * 了解业务模式
      接口业务类型、系统层次化结构
  * 性能与安全

  ### ab接口压力测试工具
  * 安装
    ```Shell
    yum install httpd-tools
    ```
  * 使用
    ```Shell 
    ab -n 2000 -c 2 http://127.0.0.1/
    -n 总的请求数
    -c 并发数
    -k 是否开启长连接
    ```