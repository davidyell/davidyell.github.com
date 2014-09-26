---
layout: post
title: "CakePHP config for Nginx"
date: 2013-08-01 15:17
comments: true
categories: [cakephp, nginx]
---

I always have trouble configuring Nginx with CakePHP as I'm forever trying to add extra things to the config.

Here is an example config template.

```nginx
server {
    listen   80;
    server_name example.dev;

    # root directive should be global
    root /Users/david/Sites/example_website/webroot;
    # If you are using a project with the `/app` folder, you'll need to uncomment the following
    # root /Users/david/Sites/example_website/app/webroot;
    access_log     /usr/local/var/log/nginx/example.access_log;
    error_log      /usr/local/var/log/nginx/example.error_log debug;
    # debug will logs lots, don't use this in production

    location / {
        index  index.php index.html index.htm;
        try_files $uri $uri/ /index.php?$uri&$args;
    }

    location ~ \.php(/|$) {
        fastcgi_pass  127.0.0.1:9001; 
        # I use port 9001 as I have XDebug on port 9000, but php-fpm uses 9000 as default
        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        include fastcgi_params;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    }
}
```