---
layout:         "default"
title:          "Cube"
lead:           ""
---

# 特点

1.  简单，容易上手。

2.  容易扩展。

3.  定制了一套规范和一套工具，方便开发。

4.  系统设计设计之初就是为着大访问量。这套架构是经过实践严格检验的。

5.  配置区分环境

# 目录结构

*   如下:

    ```bash
    +--+
       |
       +- app/                      控制台程序
       +- boot.php                  初始化文件
       +- config/                   配置文件
       +- cube/                     框架文件
       +- cube-admin/               管理后台
       |    |
       |    +- app/                 控制台工具
       |    +- cube-admin-boot.php
       |    +- htdocs/              管理后台web入口
       |    +- include/             php代码文件
       |    +- template/            管理后台模板
       |
       +- htdocs                    web可访问文件
       +- htdocs_res                js/css/image
       +- include                   php代码文件
       +- template                  模板路径
       +- writable                  日志文件，系统运行生成的临时文件
    ```

# 配置和运行

---

### 配置和初始化

*   配置`boot.php`

    如果你进行过目录调整，请修改对应的目录位置，否则仅仅修改应用名为你的应用名

*   配置 `config/engine.*.php`

    为不同的环境做不同的配置。

#### 初始化

*   配置完成之后：

    ```
    cd cube-admin/app/init
    php cube-init.php
    ```

*   开始监控资源变动

    ```
    cd cube-admin/
    sh begin-watch-res-update.sh
    ```

    到此为止，配置完成。

### 域名


你需要三个域名：

*   网站主域名:     `www.cube-php.com`
*   管理后台域名:   `admin.cube-php.com`
*   静态资源域名:   `s.cube-php.com`

#### 域名配置示例


```
server {
    listen       80;
    server_name  www.cube-php.com;

    root /home/huqiu/git/cube-php/htdocs;

    if ( $uri !~ ^/(res|static|crossDomain\.xml|robots\.txt|favicon\.ico) ) {
        rewrite ^ /index.php last;
    }
    location ~* \.(php)(.*)$ {
        fastcgi_pass        127.0.0.1:9090;
        fastcgi_index       index.php;
        fastcgi_param       SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param       PATH_INFO   $2;
        include             fastcgi_params;
    }
}

server {
    listen       80;
    server_name  admin.cube-php.com;

    root /home/huqiu/git/cube-php/cube-admin/htdocs;

    if ( $uri !~ ^/(res|static|crossDomain\.xml|robots\.txt|favicon\.ico) ) {
        rewrite ^ /index.php last;
    }

    location ~* \.(php)(.*)$ {
        fastcgi_pass        127.0.0.1:9090;
        fastcgi_index       index.php;
        fastcgi_param       SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param       PATH_INFO   $2;
        include             fastcgi_params;
    }
}

server {
    listen       80;
    server_name  s.cube-php.com;

    root /home/huqiu/git/cube-php/htdocs_res;

    include /usr/local/nginx/conf/public.conf;
}
```


