---
layout: post
title: "Installing and using pt-visual-explain"
date: 2014-08-28 17:14
comments: true
categories: ['mysql', 'percona', 'optimisation']
---
### Why?
So I attended CakeFest this year and it was brilliant. One of the talks which I took at homework was the [Profiling and Optimisation: A practical approach](https://joind.in/talk/view/11599) by [Mark Story](http://mark-story.com/).

One the tools mentioned was `pt-visual-explain`, and I've been playing with it to see how it can help me better understand my queries and what is happening when I'm getting data.

I thought that I would document the process so I don't forget it.

### Install
Go grab your specific platform download of [Percona Toolkit](http://www.percona.com/downloads/percona-toolkit/LATEST). Personally I used the **tarball** option for my work machine as it's OS X.

#### Mac OSX
Here are the instructions for installation on a Mac. I've not tried it on my Ubuntu laptop, but when I do, I'll update this post.

Then I unzipped the tarball into a folder. You'll notice that there is a `Makefile.PL`, this is a Perl script for building the various executables you'll need.

Make sure you have Perl installed, `perl --version`, which you should be default I think. Then it's time to build the various executables.

```bash
$ cd percona-toolkit-2.2.10
$ perl Makefile.PL
$ sudo make
$ sudo make test
$ sudo make install
```

Then you'll probably find some warnings or errors about a missing Perl package called `DBD::MySQL`. So let's install this. I used [this guide](http://bixsolutions.net/forum/thread-8.html), but I will summarise it here.

```bash
$ perl -MCPAN -e 'shell'
```

> If this is the first time you've used the Cpan module on your Mac you will be asked about configuring your Cpan set-up.

```bash
cpan> get DBI
cpan> get DBD::mysql
cpan> exit

$ sudo perl -MCPAN -e 'install DBI'
$ cd ~/.cpan/build/DBD-mysql-x.xxx/
$ perl Makefile.PL --testuser='daz' --testpassword='YOUR_PASSWORD'
```

Don't include the `--testpassword=''` argument if your database doesn't have a password.

```bash
$ make
$ make test
$ sudo make install
```

So now it's time to run it and see what our queries look like.

```bash
$ pt-visual-explain --connect --host=localhost --user=root --database=myDatabase ~/Percona/MyExampleQuery.sql
```

You should get a nice visual output of what is going on with your query. At this point you can add indexes, change the query and keep running the visual explainer against it to see how things are changing.

###Done
That's it! Pretty simple really, apart from all that crap with Perl ;)