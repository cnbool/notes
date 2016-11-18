title: git-添加remote url
categories:
  - git
date: 2016-05-24 10:51:44
tags:
---

博客使用hexo搭建，主题fork jacman(git@github.com:wuchong/jacman.git)，然后将我的jacman repo clone到本机
现在想将源jacman的变动更新到我的repo当中,网上查了一下可以这么干：

显示remote url
```
$ git remote -v
origin  git@github.com:cnbool/jacman.git (fetch)
origin  git@github.com:cnbool/jacman.git (push)
```

添加remote url
```
git remote add upstream git@github.com:wuchong/jacman.git
```

再看下remote url
```
$ git remote -v
origin  git@github.com:cnbool/jacman.git (fetch)
origin  git@github.com:cnbool/jacman.git (push)
upstream        git@github.com:wuchong/jacman.git (fetch)
upstream        git@github.com:wuchong/jacman.git (push)
```

现在jacman的remote url已经添加好了，fetch jacman的master分支到本地
```
$ git fetch upstream master
From github.com:wuchong/jacman
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> upstream/master
```

也可以fetch全部分支到本地
```
git fetch upstream
```

或者fetch upstream的master分支作为本地分支tmp
```
git fetch upstream master:tmp
```

合并upstream/master
```
git merge upstream/master
```

合并完成之后可以将本地文件push到自己的ropo
```
git push origin master
```



