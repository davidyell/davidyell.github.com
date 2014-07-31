---
layout: post
title: 'How to display the history of a file in Git with diffs'
categories: [git]
---

### Use case
You want to see all the changes which have been made to a single file in the project.

### References
This was solved by a brighter mind than my own.  
[Daniel Hofstetter ‏@dhofstet on Twitter](https://twitter.com/YellDavid/status/344398277275435008)

### Solution
```bash
git log -p <filename>
```

If you are using the most excellent [SourceTree](http://www.sourcetreeapp.com/) then you can select a file and go to `Actions` > `Log selected` to see a window of all the changes.

### Success

Go make a brew!