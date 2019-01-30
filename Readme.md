# ZJU Lambda 博客

## 安装 hexo

```
npm install hexo-cli -g
```

## 发布新文章

将本仓库 clone 到本地，然后使用

```
hexo new post [post name] 
```

创建新文章

## 文章元信息

文章的开头是元信息，其中 tags 是标签，若需要多个，则应该使用 markdown 中无序列表表示。category 请填自己的 id（这是一个 work around）。

```
---
title: hello
tags: 
    - others
    - nothing
category: zju-lambda group
date: 2019-01-31 01:16:50
---
```

如果文章来自其他网站，则应在元信息中增加 redirect 且在正文中写摘要。

```
---
title: hello
tags: 
    - others
    - nothing
category: zju-lambda group
date: 2019-01-31 01:16:50
redirect: https://www.github.com/nicekingwei
---

这是摘要，用于显示在主页上。
```

## 其他

也可以通过直接修改 source/_posts 文件夹来发布文章。