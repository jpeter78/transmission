vps上开webdav网盘
1. 安装nginx
```
apt-get install aptitude
aptitude install nginx nginx-extras
```
2. 创建目录，并设置权限
```
mkdir /root/download
mkdir /root/tmp
chmod 777 /root/download
chmod 777 /root/tmp
```
3. 配置
```
nano /etc/nginx/conf.d/webdav.conf
```
在里面添加内容
```
server {
    listen       17408; # 端口
    server_name  webdav.Your_domain; #域名
    root /root/download; # 根目录
    client_body_temp_path /root/tmp;
    access_log  /var/log/nginx/webdav_access.log;
    error_log   /var/log/nginx/webdav_error.log;
    location / {
      auth_basic "Not currently available";
      auth_basic_user_file /etc/nginx/conf.d/.htpasswd; # htpasswd验证文件

      dav_methods PUT DELETE MKCOL COPY MOVE;
      dav_ext_methods PROPFIND OPTIONS;

      create_full_put_path on;
      dav_access user:rw group:r;

      autoindex on; # 可以在浏览器打开
      charset utf-8,gbk; # 支持中文
   }
}
```

4. 设置密码验证
```
htpasswd -c /etc/nginx/conf.d/.htpasswd UserName #自己用户名，然后输入密码
# 可能需要安装 apache2-utils
# apt-get install apache2-utils
```

5. 安装transmission
停止transmission服务，并配置，配置文件是在/var/lib/transmission-daemon/info/settings.js
```
wget --no-check-certificate https://download.laobuluo.com/tools/debian-transmission.sh
sh debian-transmission.sh
# 一路回车，到设置用户名和密码，设置好以后，开始安装。默认端口是1989
```
6. 配置transmission
停止transmission服务，并配置，配置文件是在/var/lib/transmission-daemon/info/settings.json
```
service transmission-daemon stop
nano /var/lib/transmission-daemon/info/settings.json
# 以下是配置内容
"rpc-whitelist": "0.0.0.0",  # 默认是127.0.0.1改成0.0.0.0
"rpc-whitelist-enabled": false, # 默认是true，必须改成false，否则无法登录
```
7. transmission优化
```
wget -N --no-check-certificate https://github.com/ronggang/transmission-web-control/raw/master/release/install-tr-control-cn.sh
sh install-tr-control-cn.sh
# 回车，选9，安装以后，重新刷新后，界面就发生变化了。
```


