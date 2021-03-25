---
title: "使用Vercel部署网站"
date: 2021-03-15T23:41:24+08:00
tags:
  - "零成本快速建站"
categories:
  - "电子出版技术"
---

> 这是[估值PRO](guzhi.pro)的第2篇文章，也是[零成本快速建站](/tags/零成本快速建站/)系列（二）。
>
> 更新：2021-03-23

用[Hugo做完网站](../用静态网站生成器HUGO/)后，怎么发布到互联网上？

| 方案                                | 成本   | 缺点           |
| ----------------------------------- | ------ | -------------- |
| 虚拟空间/VPS主机                    | 高     | 要备案         |
| 国内对象储存服务+CDN（阿里、腾讯）  | 较便宜 | 要备案         |
| 国内静态网站服务商（Coding）        | 免费   | 有广告         |
| 国外静态网站服务商（Github/Gitlab） | 免费   | 百度不收录     |
| 国外CDN（Netlify/Vercel）           | 免费   | 限每月100G流量 |

> 如果有已备案域名，可以考虑国内对象储存，不然还是Vercel吧！

<!--more-->

不理解的可以再看看下图。

![网站部署示意图](%E7%BD%91%E7%AB%99%E9%83%A8%E7%BD%B2%E7%A4%BA%E6%84%8F%E5%9B%BE.png)

在国内提供服务最好还是备案，但备案必须有国内主机，国内主机再便宜也要400一年，加上域名费用，没有盈利能力的网站，还是能省则省，等~~上了正轨~~主机大促再投入不迟。

静态网站一般选择是托管到Github，但Github国内速度慢且百度不收录，所以托管到Github以后还要通过CDN绕一下。

以前大家部署静态网站，一般用[Netlify](https://www.netlify.com/)。[Netlify](https://www.netlify.com/)主要提供持续集成（提交代码到Github后，自动从Github拉取代码、生成页面、编译程序）和CDN加速服务。

但同类网站[Vercel](https://vercel.com/)对中国境内的访问速度优化更好，免费账户限100G流量，所以逐渐都转向Vercel。

## 一、把网站提交到Github

注册Github账户和在GitHub创建仓库（Repository）不再赘述。

创建完Github仓库后，在本地终端中按以下命令操作:

```bash
# 如果未初始化过，初始化GIT仓库 && 添加所有文件监视 && 提交第一次修改
git init && git add . && git commit -m "1st"
# 设置主分支为main（2021年后GITHUB政治要求）
git branch -M main
# 新增远程分支
git remote add origin https://github.com/[你的账号]/[你的repository].git
# 如果远程仓库原来不为空仓库，拉取origin远程分支至本地main，合并不相关的历史分支。如果为空，不需要以下命令
git pull origin main --allow-unrelated-histories && git commit -m "merge remote"  
# 推送main分支到远程分支origin
git push -u origin main
```

不喜欢提交时输入账号密码的，可以改SSH方式提交（需要创建密钥和设置SSH，密钥和SSH详见[Linux服务器管理基础](../Linux服务器管理基础/)）

```bash
# 在本机生成密钥对，然后在GitHub中添加公钥，网址：
https://github.com/settings/keys

# 在config文件中，指定公钥所对应的私钥
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

## 二、从Vercel拉取网站并绑定域名

### （一）导入站点

Vercel 的使用十分简单，只需要使用 GitHub 登录，然后填写源码所在的 repository 地址即可，会自动识别使用的框架，并自动生成静态文件后部署。Vercel 会自动分配给你一个网址，用来预览效果。

### （二）自定义域名

在项目主页点击 **Settings -> Domains** 可以添加自己的域名，由于想要使用 Vercel 自带的 CDN 服务，把域名的 DNS 记录指向 Vercel 即可，同时设置仅 DNS。

---

至此，一个静态网站就成功部署到互联网了。

可以通过测速网站测试一下网站速度（[17ce.com](17ce.com)/ [boce.com](boce.com)/[ce8.com](ce8.com)）。

美观方面，请继续看 [Hugo网站美化进阶](../hugo网站美化进阶/)。

FIN.





