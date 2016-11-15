---
layout: post
title: "Sass required asterisk partial"
comments: true
categories: [sass, cakephp]
date: 2012-12-18 09:43:25
---

So I am forever looking up this code.  

### Code
`_required.scss`  

```scss
div.input{
    &.required label:after{
        content: "*";
    }
    &.error{
        padding: 10px;
        color: #B94A48;
        background-color: #F2DEDE;
        border: 1px solid #EED3D7;
        border-radius: 4px;
    }
}
```

This will add a star after any required fields, and also give a nice error style.