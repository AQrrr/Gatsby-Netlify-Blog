---
title: 用Gatby和Netlify创建个人网站
date: "2020-11-16T23:46:37.121Z"
---


费了好大劲做出的一个toy project，使用Gatby和Netlify部署个人站点，可以做成个人博客或者在线简历。中文网页里关于Gatby和Netlify的信息不太多，希望可以帮到有兴趣尝试的同学。
项目代码在我的[Git](https://github.com/AQrrr/GatsbyDemo).



Gatsby 是一个基于 React 的免费、开源框架，用于帮助开发者构建运行速度极快的网站和应用程序。Netlify 是一个提供静态资源托管的网络平台。

本教程使用gatsby官网提供的一页式网站模板，这里是原作者git链接 [lekoarts/gatsby-theme-cara](https://github.com/LekoArts/gatsby-themes/tree/master/themes/gatsby-theme-cara).  


大家也可以去[Gatsby Starter Library](https://www.gatsbyjs.com/starters/?v=2)里找自己喜欢的其他模板，之后复制模板中的命令创建文件就可以。

准备工作：
1. 在Gatsby和Netlify完成注册
2. 已经安装git
3. 很多很多耐心，以及善用Google 😂






## 🚀 准备开始

1. **创建Gatsby站点**
请先去Gatsby官网完成注册，之后全局安装Gatsby 
```sh
npm install -g gatsby-cli
```
然后用Gatsby CLI创建新站点

```sh
gatsby new (你的文件夹名字) https://github.com/LekoArts/gatsby-starter-portfolio-cara
```
ps. 创建新站点这一步，示例中我选用的是gatsby-starter-portfolio-cara，大家可以换成自己喜欢的starter

2. **开始运行**

进入你的文件夹，开始运行

```sh
cd 你的文件夹名字

用npm
npm install // 安装依赖 要等好久……

或者用yarn
yarn // 安装依赖 要等好久……

最后
gatsby develop
```

3. **创建完毕**

可以打开浏览器查看你的网页啦 `http://localhost:8000`


## 🚀 Netlify配置
4. **添加Netlify CMS**

安装依赖：
```sh
npm install --save netlify-cms-app gatsby-plugin-netlify-cms
```

##### 配置
我们需要从github仓库部署到Netlify，因此要做一些基本配置。

在static/admin文件夹里新建config.yml文件。 目录结构如下：
```sh
├── static
│   ├── admin
│   │   ├── config.yml
```

把以下代码粘贴到config.yml中：
```sh
backend:
  name: git-gateway
  branch: main

media_folder: static/img
public_folder: /img

collections:
  - name: 'blog'
    label: 'Blog'
    folder: 'content/blog'
    create: true
    slug: 'index'
    media_folder: ''
    public_folder: ''
    path: '{{title}}/index'
    editor:
      preview: false
    fields:
      - { label: 'Title', name: 'title', widget: 'string' }
      - { label: 'Publish Date', name: 'date', widget: 'datetime' }
      - { label: 'Description', name: 'description', widget: 'string' }
      - { label: 'Body', name: 'body', widget: 'markdown' }
```

在`gatsby-config.js`中，添加插件 <br/>

```sh
plugins: [`gatsby-plugin-netlify-cms`]
```
##### 本地文件上传到git仓库
在你的Github新建一个仓库，然后完成上传
```sh
git init
git add .
git commit -m "Initial Commit"
git remote add origin https://github.com/YOUR_USERNAME/NEW_REPO_NAME.git
git push -u origin main
```
## 🚀 Netlify部署
**5. 部署到Netlify**

登录Netlify点击 'New Site from Git'. 

##### · 开启 Identity 和 Git Gateway
开启后，你的网站主账户可以从Netlify管理后台，不需要反复从git进行修改。 

在Netlify的site dashboard中:

点击 **Settings > Identity**, 选择 Enable Identity service.
在 **Registration preferences**, 选择 Open 或者 Invite only.  
左侧**Services > Git Gateway**, 选择 **Enable Git Gateway**. 
![](https://img-blog.csdnimg.cn/img_convert/6426e6a5b632a4f6cd9e2f96e7ce95a0.png)
##### · 更新你的网站
登录你的网址 /admin/  点击 New Blog 发布新文章


***


我创建的[demo在这里](https://inspiring-demo.netlify.app/)


**参考资料：**

Netlify[官网指南](https://www.netlifycms.org/docs/gatsby/#get-to-know-gatsby)

Gasby[官方教程](https://www.gatsbyjs.cn/tutorial/)