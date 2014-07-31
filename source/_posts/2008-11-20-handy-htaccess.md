---
layout: post
title: 'Handy htaccess rules'
categories: [htaccess, apache]
---

I don't tend to use Apache these days, having swapped over to using Nginx, but these always caught me out.  

```apache
# Pass all the /forums stuff through the routing
RewriteCond %{REQUEST_URI} "/forums/"
RewriteRule (.*) $1 [L]

# If you need to lock down access to a folder, such as a CMS, then use the following
RewriteRule ^cms(.*)$ /maintenance.html [R=301,L]

# Redirecting everyone to maintenance
# Reference, http://www.techiecorner.com/97/redirect-to-maintenance-page-during-upgrade-using-htaccess/
RewriteCond %{REQUEST_URI} !/maintenance.html$
RewriteRule $ /maintenance.html [R=302,L]
```