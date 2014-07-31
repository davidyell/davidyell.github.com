---
layout: post
title: 'Nginx config for Symfony2'
categories: [symfony2, nginx]
---

Wow, so I've just spent the last 2.5 hours trying to configure nginx to work with my Symfony2 project and I've finally cracked it, so this is a celebratory post to ensure that I don't have to look for the config again.

```nginx
server {
  listen 80;
 
  server_name tabletennistracker.dev;
  root /home/neon/Sites/TableTennisTracker/web;
 
  error_log /var/log/nginx/tabletennistracker.error.log;
  access_log /var/log/nginx/tabletennistracker.access.log;

  location / {
    index app.php;
    try_files $uri $uri/ /app.php?$query_string;
  }
 
  location ~ ^/(app|app_dev)\.php(/|$) {
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_pass 127.0.0.1:9001;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
  }
}
```

The [nginx symfony page](http://wiki.nginx.org/Symfony) had some strange `@rewrite` in it which was causing my server to fail on a redirect loop. This turned out to be the config which worked.

Time for another glass of wine!