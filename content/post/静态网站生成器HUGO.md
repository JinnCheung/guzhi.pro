---
title: "静态网站生成器Hugo"
date: 2021-03-15T18:16:24+08:00
tags:
  - "零成本快速建站"
categories:
  - "电子出版技术"
---

> 这是[估值PRO](guzhi.pro)的第1篇文章，也是[零成本快速建站](/tags/零成本快速建站/)系列的第1篇文章。
>
> *2021-03-15*

[Hugo](https://gohugo.io/)是一款跨平台静态网站生成器，采用[golang](https://golang.org/)编写，优点主要有：

**运维成本低**：与WordPress、Drupal等传统博客程序相比，Hugo不需要管理数据库和后台程序，几乎不占用计算资源。

**开发效率高**：与hexo、jekyll等静态网站生成器相比，Hugo页面渲染速度更快，支持本地自动刷新，没有复杂的第三方依赖。

**学习成本低**：Hugo已经提供300+款主题模板，支持windows, macOS, linux跨平台，配置较为简单。

<!--more-->


## 一 安装

### （一）安装Hugo

Hugo安装方法详见[官方文档·安装](https://gohugo.io/getting-started/installing/)，主要有直接下载可执行文件、Docker镜像、包管理器、编译安装等几种方式。

推荐直接使用包管理器安装，如macOS下的homebrew，在终端下输入：

```bash
brew install hugo
```

安装完成后，新建网站“MySite”

```bash
hugo new site MySite
```

### （二）安装主题

以[Binario](https://github.com/vimux/binario/)主题为例：

```bash
cd MySite # 进入MySite根目录
git init  # 初始化GIT仓库
git submodule add https://github.com/vimux/binario themes/binario # 下载主题到 themes/ 文件夹
```

下载主题后，在网站配置文件 `config.toml` 中添加 `theme = "binario"`.

也可以直接复制主题样例文件到根目录，里面包含主题预设的`config.toml`和其他内容范例文件。

```bash
cp -r themes/binario/exampleSite/* . #不要漏了最后那个点
```

### （三）启动服务器

在网站根目录运行`hugo server`，启动本地hugo网站服务器，就可以通过 [http://localhost:1313](http://localhost:1313/) 来访问网站。

下面是`hugo server`常用的参数, 注意大小写：

- `-p 端口`: 修改默认端口
- `-D`: 使用server预览网站时, draft属性为ture的草稿文件是不会生成预览的，添加-D后可以预览草稿文件。

### （四）新建文章

通过以下命令在`content`目录中新建文章页面，以 *测试页* 为例：

```bash
hugo new post/测试页.md
```

该页面是根据`archetypes/default.md`模板文件生成的，默认状态为草稿，预览的话，需要`hugo server -D`，或者在 *测试页* 的Front Matter信息中将draft属性改成false

```yaml
---
title: "测试页"
date: 2021-03-15T18:16:24+08:00
draft: false
---
```

### （五）生成网站

在网站根目录中输入`hugo`命令，不带任何参数，则默认`/public`目录下面生成静态网站文件。

```bash
Start building sites … 

                   | ZH  
-------------------+-----
  Pages            | 57  
  Paginator pages  |  1  
  Non-page files   |  2  
  Static files     | 13  
  Processed images |  0  
  Aliases          | 23  
  Sitemaps         |  1  
  Cleaned          |  0  

Total in 95 ms
```

## 二、配置

### （一）主导航

在配置文件 `config.toml` 中添加：

```toml
[menu]
  [[menu.main]]
  name="标签"
  url="/tags/"
  weight="1"
  [[menu.main]]
  name="分类"
  url="/categories/"
  weight="2"
  [[menu.footer]]
  name="归档"
  url="/achieves/"
  weight="1"
```

以上代码表示在主导航（menu.main），添加 *标签* 和 分类 两个链接，在底部导航（menu.footer），添加 *归档* 链接

也可以直接在页面的Front_Matter信息中，添加一行`menu: main`或`menu.footer`，指定该页面加进导航中

```yaml
---
title: About Hugo
description: Hugo, the world’s fastest framework for building websites
date: 2019-02-28
author: Hugo Authors
menu: footer
---
```

### （二）中文化

Hugo支持多语言设置，详见[官方文档·多语言](https://gohugo.io/content-management/multilingual/)，这里只介绍如何将英文的[Binario](https://github.com/vimux/binario/)主题修改为中文。

#### 修改多语言文件

在配置文件 `config.toml` 中添加：

```toml
defaultContentLanguage = "zh"
defaultContentLanguageInSubdir = true
```

然后在`themes/binario/i18n`文件夹下，将`en.yaml`复制一份，重命名为`zh.yaml`，修改`zh.yaml`对应的`translation`项即可。

#### 修改默认列表页模板

由于Hugo默认生成的两种文章分类法（Taxonomy）——分类（Categories）和标签（Tags）的页面是自动生成的，因此这两个分类法的列表页没有办法通过翻译，我们可以通过建立这两种分类法的模板文件，来解决这个问题。

分别在`content/`文件夹下，建立`categories/`文件夹和`tags/`文件夹，两个文件夹下分别建立`_index.md`文件，内容如下：

```yaml
---
title: "分类"
---
```

`post/`文件夹也可以用同样的操作设置列表页的自定义标题

### （三）面包屑导航

修改`config.toml`

```toml
[Params.Breadcrumb]
  enable = true # 开启全站面包屑导航
  homeText = "估值PRO" # 设置首页导航名称
```

不需要的页面，可以在页面的Front_Matter信息中关掉。

```yaml
---
breadcrumb: false
---
```

至此，一个静态网站就在本地搭建好了。

发布到互联网方面，请查看 [通过Netlify部署静态网站](../通过netlify部署静态网站/)

美观方面，请继续看 [Hugo网站美化进阶](../hugo网站美化进阶/)

FIN.

