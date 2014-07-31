---
layout: post
title: 'How to remove all .svn folders from a repo'
categories: [svn, bash]
---

So when you accidentally upload a dev folder controlled by subversion you can strip out all svn information.

```bash
find . -name ".svn" -type d -exec rm -rf {} \;
```

Reference from, [http://snippets.dzone.com/posts/show/2486](http://snippets.dzone.com/posts/show/2486)