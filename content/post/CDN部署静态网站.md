---
title: "通过CDN部署静态网站"
date: 2021-03-15T23:41:24+08:00
tags:
  - "零成本快速建站"
categories:
  - "电子出版技术"
---

> 这是[估值PRO](guzhi.pro)的第2篇文章，也是[零成本快速建站](/tags/零成本快速建站/)系列的第2篇文章。
>
> *2021-03-16*

用[Hugo做完网站](../用静态网站生成器HUGO/)后，怎么发布到互联网上？

| 方案                                | 成本   | 缺点           |
| ----------------------------------- | ------ | -------------- |
| 虚拟空间/VPS主机                    | 高     | 要备案         |
| 国内对象储存服务+CDN（阿里、腾讯）  | 较便宜 | 要备案         |
| 国内静态网站服务商（Coding）        | 免费   | 有广告         |
| 国外静态网站服务商（Github/Gitlab） | 免费   | 百度不收录     |
| 国外静态网站CDN（Netlify/Vercel）   | 免费   | 限每月100G流量 |

> 如果有已备案域名，可以考虑国内对象储存，不然还是Netlify吧！

<!--more-->

## 一、把网站提交到Github

注册Github账户和在GitHub创建仓库（Repository）不再赘述。

在终端中，在网站文件夹下按以下命令操作:

```bash
# 如果未初始化过，初始化GIT仓库 && 添加所有文件监视 && 提交第一次修改
git init && git add . && git commit -m "1st"
# 设置主分支为main（2021年后GITHUB政治要求）
git branch -M main
# 新增远程分支
git remote add origin https://github.com/[你的账号]/[你的repository].git
# 如果远程仓库原来不为空仓库，拉取origin远程分支至本地main，合并不相关的历史分支。如果为空，忽略
git pull origin main --allow-unrelated-histories && git commit -m "merge remote"  
# 推送main分支到远程分支origin
git push -u origin main
```

不喜欢提交时输入账号密码的，可以改SSH方式提交（需要创建密钥和设置SSH，密钥和SSH详见[Linux服务器管理基础](../Linux服务器管理基础/)）

```bash
# 在本机生成密钥对，然后在GitHub中添加公钥
https://github.com/settings/keys

# 在config文件中，指定github.com所对应的私钥
nano ~/.ssh/config
---
Host github.com
        HostName github.com
        IdentityFile ~/.ssh/[你的私钥]
---        
# 删除之前的HTTPS远程仓库
git remote remove origin 
# 添加SSH远程仓库
git remote add origin git@github.com:[你的账号]/[你的repository].git
# 推送远程仓库
git push -u origin main
```

## 二、注册Netlify并链接到GitHub的Repository

基本就是点[New site from Git](https://app.netlify.com/start)，然后选GItHub然后无脑点下一步就行了。

配置域名。就搞好了。

由于 GitHub Pages 在国内的访问速度实在不理想，使用 cloudflare CDN 也没什么太好的效果，因此一直在寻找合适的替代品。最近了解到 [Vercel](https://vercel.com/) 同样提供了简单的一键式托管服务，同时在国内访问速度十分优良，因此迁移到了该服务。下面简单记录一下折腾的过程。



# 一键托管

## 导入站点

Vercel 的使用十分简单，只需要使用 GitHub 登录，然后填写源码所在的 repository 地址即可，会自动识别使用的框架，并自动生成静态文件后部署。Vercel 会自动分配给你一个网址，用来预览效果。

## 自定义域名

在项目主页点击 **Settings -> Domains** 可以添加自己的域名，由于想要使用 Vercel 自带的 CDN 服务，把域名的 DNS 记录指向 Vercel 即可，同时设置仅 DNS。

# 自定义部署

使用 Vercel 的全自动部署固然方便，然而会遇到一些小问题，比如[这里](https://editio.me/2019/hexo-CI-github-actions/)提到的网页时间问题，需要先处理一下文件的时间。此外还可以自定义404页面来替换丑丑的 Vercel 默认404页面。

## 自定义部署命令

Vercel 默认读取的是根目录中 `package.json` 的部署命令，因此想要修改部署时执行命令的话可以先在根目录添加一个 `vercel.sh` 文件，写好自定义命令。

```
#!/bin/bash

export TZ='Asia/Shanghai'
git ls-files -z | while read -d '' path; do 
    touch -d "$(git log -1 --format="@%ct" "$path")" "$path";
done
hexo generate
```

之后修改 `package.json` 中的 `scripts` 部分。

```
"scripts": {
  "build": "bash ./vercel.sh",
  "clean": "hexo clean",
  "deploy": "hexo deploy",
  "server": "hexo server"
},
```

这样 Vercel 在部署时就会执行 `vercel.sh` 的命令了。

## 自定义404页面

参考 Vercel 的[官方文档](https://vercel.com/docs/configuration#project/routes)，我们可以使用 routers 功能把404状态的请求指向我们自己的404页面。

在根目录下添加 `vercel.json` 文件，填入以下内容即可。

```
{
    "version": 2,
	"routes": [
	  { "handle": "filesystem" },
	  { "src": "/(.*)", "status": 404, "dest": "/404.html" } 
	]
}
```

此外vercel还支持 Serverless 功能，包括 Node.js, Go, Python, Ruby 语言，可以轻松开发自己想要的玩法。

## 三、设置图片CDN





