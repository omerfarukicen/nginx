# Introduction

Nginx is one of the most popular web servers in the world and is responsible for hosting some of the largest and highest-traffic sites on the internet.
it provides many important capabilities including:
- Reverse Proxy
- Load Balancer
- Web-Application Firewall


## Installing

```shell
sudo apt update
sudo apt install nginx
```

### Adjusting The Firewall

```shell
sudo ufw app list
```
- Nginx Full: This profile opens both port 80 (normal, unencrypted web traffic) and port 443 (TLS/SSL encrypted traffic)
- Nginx HTTP: This profile opens only port 80 (normal, unencrypted web traffic)
- Nginx HTTPS: This profile opens only port 443 (TLS/SSL encrypted traffic)


```shell
 sudo ufw allow 'Nginx HTTP'
 sudo ufw status
```

## Managing Nginx Process
```shell
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl reload nginx
sudo systemctl disable nginx
sudo systemctl enable nginx
```
## Configuration Contexts
- main
- events
- http
- mail

#### Main Context
Any directives that exist entirely outside of context blocks is said main context

     user www-data;
     worker_processes auto;
     pid /run/nginx.pid;
     include /etc/nginx/modules-enabled/*.conf;


#### Event Context

it is used to set global options that affect how Nginx handles connections at a general level. There can only be a single events context defined within the Nginx configuration.

     events {
	      worker_connections 768;
	      # multi_accept on;
      }

#### HTTP Context

Defining an HTTP context is probably the most common use of Nginx. When configuring Nginx as a web server or reverse proxy, the “http” context will hold the majority of the configuration. This context will contain all of the directives and other contexts necessary to define how the program will handle HTTP or HTTPS connections.

     events {
	      worker_connections 768;
	      # multi_accept on;
      }
      

## Nginx Configuration
https://www.digitalocean.com/community/tools/nginx

#### Test configuration file
```shell
nginx - t 
```

## Reverse Proxy
```shell
cd /etc/nginx/conf.d
nano proxy.conf
server {
    listen       80;
    server_name  localhost;
 
    location / {
        proxy_pass http://10.139.0.3;
    }
 
    location /admin {
        proxy_pass http://139.59.11.125;
      }
}
nginx -t
systemctl restart nginx

```
### Reverse Proxy move Header
```shell
    proxy_set_header Host github.com.tr;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header        X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-For $remote_addr;
    proxy_set_header Origin https://test-intranet.hmb.gov.tr;
    proxy_pass http://github_backend_servers/;
    proxy_hide_header Access-Control-Allow-Origin;

```
 Reverse Proxy Side
 ```shell
  nano /etc/nginx/conf.d/proxy.conf
  proxy_set_header X-Real-IP $remote_addr;
```		 
 Backend Server Side
 ```shell
   nano /etc/nginx/nginx.conf
   "$http_x_real_ip"
 ```	


## Implementing Nginx as Load Balancer 
 ```shell
nano loadbalancer.conf
 ```	
 
 ```shell
upstream backend {
  server 10.139.0.3;
  server 10.139.0.4;
}
 
server {
    listen       80;
    server_name  localhost;
 
    location / {
        proxy_pass http://backend;
 }
}
 ```	
### Caching
 ```shell
    location ~ \.(jpeg){
     root  /usr/share/nginx/html/kplabs;
#     expires 20s;
     add_header Cache-Control "no-cache";
     add_header Cache-Control "no-store";
     add_header Cache-Control "must-revalidate";
}
 ```	


[TOC]

## Welcome
Welcome to Write.md, a place to write Markdown without any distractions. Your document is secured using AES 256 bit encryption and you can quickly share and collaborate using the URL in your address bar.

## Sharing & Collaboration
You can share this doc using https://writemd.xyz/d/63a6bce3e9e8f5414. Anyone that has access to your document URL can make changes that save/overwrite the original content, so be careful who you share with.

## Preview
You can enter preview of your document using https://writemd.xyz/p/63a6bce3e9e8f5414 - replacing the D for document in the URL with P for preview.

## Export & Print
You can export or print your document by firstly previewing the page (see Preview) and using your browsers default printing option which should allow you to export PDF, HTML or send to printer.

## Variables (new)

--var family_name: Dursley

When Mr and Mrs {{family_name}} woke up on the dull, grey Tuesday our story starts, there was nothing about the cloudy sky outside to suggest that strange and mysterious things would soon be happening all over the country. Mr {{family_name}} hummed as he picked out his most boring tie for work and Mrs {{family_name}} gossiped away happily as she wrestled a screaming Dudley into his high chair.

## Themes

You can set both the editor and preview pane theme by creating a new variable called `wmdEditorTheme` and `wmdPreviewTheme` and setting them anywhere in the page.

Heres an example of the default look, remove the space between -- and var to set.

```
-- var wmdPreviewTheme: default
-- var wmdEditorTheme: dark
```

The available themes are as follows:

### Editor Themes
- default
- 3024-day
- 3024-night
- ambiance
- ambiance-mobile
- base16-dark
- base16-light
- blackboard
- cobalt
- eclipse
- elegant
- erlang-dark
- lesser-dark
- mbo
- mdn-like
- midnight
- monokai
- neat
- neo
- night
- paraiso-dark
- paraiso-light
- pastel-on-dark
- rubyblue
- solarized
- the-matrix
- tomorrow-night-eighties
- twilight
- vibrant-ink
- xq-dark
- xq-light

### Preview Themes
- default
- dark

## Flow Charts
```flow
st=>start: Launch Write.md
op=>operation: Start Writing
cond=>condition: Success? 
e=>end: Awesome

st->op->cond
cond(yes)->e
cond(no)->op
```

## What is Markdown?
Markdown (MD) is a lightweight markup language with plain-text-formatting syntax. Its design allows it to be converted to many output formats, but the original tool by the same name only supports HTML. If you would like to see the cheatsheet for writing MD just click [here](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

## Love Write.md?
Why not leave us a review on product hunt to let others know how much you enjoy using Write.md https://www.producthunt.com/posts/write-md
