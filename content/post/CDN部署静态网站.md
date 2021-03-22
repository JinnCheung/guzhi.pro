---
title: "通过Netlify部署静态网站"
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
| 国外静态网站CDN（Vercal）           | 免费   | 限每月100G流量 |

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



