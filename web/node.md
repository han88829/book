1.首先你得安装`node.js`，我是用`nvm`安装的，这样比较好控制版本，当然你也可以使用`apt-get`。

2.下一步是安装`Nginx`，不去管版本的话，直接`sudo apt-get install nginx`就行。

3.进入`/etc/nginx`目录，查看`nginx.conf`配置文件，在`http`块中找到这样两句：

```
# include /etc/nginx/conf.d/*.conf;
# include /etc/nginx/sites-enabled/*;
```

看看你的这两句有没有注释掉，如果注释了就把`#`号去掉，没有注释的话就跳过这一步。

4.进入`/etc/nginx/conf.d`目录，创建我们自己的配置文件，去名规则最好是域名加端口，这样以后方便找，比如我的：`rockjins-com-8081.conf`，配置文件写入以下内容：

```
upstream rockjins {
    server 127.0.0.1:8081; # 这里的端口号写你node.js运行的端口号，也就是要代理的端口号，我的项目跑在8081端口上
    keepalive 64;
}

server {
    listen 80; #这里的端口号是你要监听的端口号
    server_name 39.108.55.xxx www.rockjins.com rockjins.com; # 这里是你的服务器名称，也就是别人访问你服务的ip地址或域名，可以写多个，用空格隔开

    location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-Nginx-Proxy true;
        proxy_set_header Connection "";
        proxy_pass http://rockjins; # 这里要和最上面upstream后的应用名一致，可以自定义
    }
}
```

5.保存文件后，输入`sudo nginx -t`测试我们的配置文件是否有错误，一般错误都是漏个分号，少个字母之类的，错误提示很精确，没错的话会输出下面两句:

```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

6.现在我们需要重启`Nginx`，我们的配置文件才会生效，输入`sudo service nginx reload`;

