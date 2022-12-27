# First Configuration
## Download
## Configuration
```shell
./configure --prefix=/usr/share/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --http-client-body-temp-path=/var/lib/nginx/tmp/client_body --pid-path=/var/run/nginx.pid --lock-path=/var/lock/subsys/nginx --user=nginx --group=nginx --with-http_mp4_module --add-module=../nginx-hello-world-module

```
```shell
useradd Nginx
mkdir -p /var/lib/nginx/tmp/
chown -R nginx.nginx /var/lib/nginx/tmp/
```
Save this file as  /lib/systemd/system/nginx.service

```shell
[Unit]
Description=The NGINX HTTP and reverse proxy server
After=syslog.target network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/run/nginx.pid
ExecStartPre=/usr/sbin/nginx -t
ExecStart=/usr/sbin/nginx
ExecReload=/usr/sbin/nginx -s reload
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

# Third Party Module

```shell
git  clone perusio/nginx-hello-world-module

./configure --add-dynamic-module=/path/to/nginx-hello-world-module

make modules

```



```shell

```



```shell

```
