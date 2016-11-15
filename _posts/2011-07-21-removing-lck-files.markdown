---
layout: post
title: 'How to remove all .lck files from a folder'
comments: true
categories: [dreamweaver, bash]
date: 2011-07-21 09:43:25
---

So if you have ever worked on a project and someone is using Dreamweaver as their IDE and uploading all the `.lck` files to the server, you'll want this.  

```bash
find . -name "*.LCK" -exec rm {} \;
```

Ref: [http://www.mediacollege.com/adobe/dreamweaver/site-management/lck-remove.html](http://www.mediacollege.com/adobe/dreamweaver/site-management/lck-remove.html)