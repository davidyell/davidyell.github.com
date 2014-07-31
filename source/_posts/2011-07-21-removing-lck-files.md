---
layout: post
title: 'How to remove all .lck files from a folder'
categories: [dreamweaver, bash]
---

So if you have ever worked on a project and someone is using Dreamweaver as their IDE and uploading all the `.lck` files to the server, you'll want this.  

```bash
find . -name "*.LCK" -exec rm {} \;
```

Ref: [http://www.mediacollege.com/adobe/dreamweaver/site-management/lck-remove.html](http://www.mediacollege.com/adobe/dreamweaver/site-management/lck-remove.html)