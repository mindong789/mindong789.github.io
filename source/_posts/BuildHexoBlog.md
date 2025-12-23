---
title: 如何搭建一个静态站
date: 2025-11-27 10:02:34
cover: [https://copyright.bdstatic.com/vcg/creative/1bb2ffd939ee16571df6506cf29693e4.jpg]
sticky: 0
banner:
  type: img
  bgurl: https://t7.baidu.com/it/u=2507897800,3826496196&fm=193
  banner_text: Hi my new friend!
author: 小东
tags: [Hexo, Blog]
categories: 技术
---
这篇文章主要介绍如何来搭建一个属于自己的静态站。

> **注：本文是 mac 环境的教程，其他环境可以借鉴，无法照抄**

# 前提
本篇文章主要分为 `hexo` 、`github` 、`cloudflare` 三个部分，大概的内容就是介绍组件和如何使用，如果按照本文的步骤操作遇到错误时，肯定是环境问题导致的，可以咨询 `AI` 来解决。

推荐 `AI` ：<span style="color:rgba(236, 184, 132, 0.92)">[豆包](https://www.doubao.com/chat/)</span> <span style="color:rgba(68, 222, 119, 0.93)">[千问](https://www.qianwen.com/)</span>

---
<span style="color:#9370db; font-size:1.2em; font-style:italic;">接下来开始建站旅程吧！</span>

----
# Hexo
Hexo 是一款快速、简洁且高效的静态博客框架，基于 Node.js 构建，能让你通过 Markdown 轻松编写博客并生成静态网页。

### 环境准备
hexo 依赖Node.js 和 Git 两个工具，需要提前安装：

1. 安装 Node.js
```js
# 安装
brew install node # 安装 Node.js
npm install -g npm   # 更新 npm 自身 (Node.js 的包管理工具)

# 安装后验证
node -v
npm -v
```
2. 安装 Git

```js
# 安装
brew install git

# 验证
git --version
```
3. 安装 Hexo CLI

```js
npm install -g hexo-cli
```
### 本地搭建
1. 初始化博客项目

```js
hexo init my-blog # ”my-blog“ 自己的博客目录名称
cd my-blog
npm install
```
2. 生成一篇博客
```js
hexo new "我的第一篇博客"
```
文章默认保存在 source/_posts/ 目录下，格式为 Markdown（.md）。

3. 编辑文章内容

```js
---
title: 我的第一篇博客
date: 2025-11-21 11:40:00
tags: [Hexo, Blog]
categories: 技术
---

这是我的第一篇 Hexo 博客！🎉
```
4. 本地预览

```js
hexo server
```
然后在浏览器访问：http://localhost:4000
> 可加 -d 参数指定端口：hexo server -p 3000

5. 更换主题（可选）

Hexo 默认主题比较简单，可以更换为其他美观的主题。

主题列表：[Hexo主题](https://hexo.io/themes/)
    
```js
# 安装主题
# 在自己选定的主题的 git 中按照教程安装
# 本文以 hexo-theme-async 主题作为案例。
# https://github.com/MaLuns/hexo-theme-async
cd yourHexoFolder # 确保在你的项目目录
npm install --save hexo-theme-async hexo-renderer-less hexo-renderer-ejs

# 如果需要更新主题
npm install hexo-theme-async@latest
```
然后修改 `_config.yml`（站点配置文件）中的 `theme` 字段：

```js
theme: async
```

6. 美化站点

按照主题介绍文档进行配置
```js
hexo server
```
启动站点，查看是否符合预期

# Github
GitHub 是全球最大的基于 Git 的代码托管与开发者协作平台，也是活跃的开源社区。
本文搭建的站点使用 Github 作为代码托管仓库和构建打包发布平台。

[Github站点](https://github.com/)

### 创建仓库
在 Github 上创建一个仓库，命名格式：`<username>.github.io`

### 安装插件

```js
npm install hexo-deployer-git --save
```
### 配置 git 信息

配置 `_config.yml` 中的 `deploy` 部分：

```js
deploy:
  type: git
  repo: https://github.com/<username>/<username>.github.io.git
  branch: main
```
### 生成静态文件&构建
```js
hexo clean && hexo generate && hexo deploy
```
这里有可能会 `deploy` 失败，因为高版本的 `git` 不支持输入密码。此时需要再 `Github` 上生成一个可写入的 `token` 并使用 `git` 命令指向该仓库。

```js
git init
git remote add origin https://github.com/<username>/<username>.github.io.git
git add .
git commit -m "init"
git push origin main

# 上述命令成功后重新执行构建部署
hexo clean && hexo generate && hexo deploy
```
### 开启 Github Pages
需要手动开启 GitHub Pages：
1. 打开 GitHub 仓库 → 点击顶部的 Settings；
2. 左侧选择 Pages；
3. 在 Build and deployment 下的 Source 选择 Deploy from a branch；Branch 选择 gh-pages（或你配置的分支），点击 Save；
4. 等待几分钟后，访问 `https://<用户名>.github.io/<仓库名>/` 即可。

### 配置域名
Github 提供站点国内访问速度较慢，如果觉得不满意，可以自定义国内域名。（<font color="orange">如何设置就自己百度吧~。~</font>）

> **基于 `Github` 搭建的个人站点就成功了，可以尽情访问啦~**

# Cloudflare
Cloudflare 是一个负责给网站加速、抵御攻击、流量管理、域名管理和开发支持的平台，在全球都有数据中心，相比于Github，Cloudflare提供的站点访问速度更快，提供的功能更多。

[Cloudflare站点](https://dash.cloudflare.com/)

### 新建 Pages 项目
1. 登录 Cloudflare 控制台 → 左侧菜单选择「Workers & Pages」→ 点击「Create a application」；
2. 选择「Connect to Git」，授权 Cloudflare 访问你的 GitHub 账号（需登录 GitHub 确认授权）；
3. 选择 Hexo 博客所在的 GitHub 仓库 → 点击「Begin setup」。
4. 填写构建参数

```js
Build Command：
npm install && npx hexo clean && npx hexo generate

Build output:
public
```

> 配置完成后，点击「Save and Deploy」，Cloudflare 会自动拉取 GitHub 源码、执行构建命令，并部署到 Pages。

### 自动构建
需要本地 `Hexo` 项目的源码 `push` 到 `Github` 上，不能使用 `hexo deploy` 命令了。

`git` 指令：

```js
git init 
git add .
git commit -m "init"
git push origin main
```

> 接下来就可以观赏自己的站点了，链接为 <项目名>.pages.dev 。请自己进行尝试吧~

### 自定义域名
需要自己购买域名并托管到 `Cloudflare` 上，进行关联，咨询 `AI` 即可，比较简答。在本文就不进行介绍了~

# 结束语
嘿嘿~，还是比较开心的，毕竟是第二篇自己写的博客，虽然不说写的有多好，但还是耗费了一些心血的。希望可以帮助大家建立起属于自己的博客！

本文中大多数知识咨询 AI 即可获取，如果没有学习过前端知识的话，大多数困难都集中在主题的配置，尽量阅读介绍文档，里面包含大多数答案。

> 通过本文，您已掌握 Hexo + GitHub Pages + Cloudflare 的完整搭建链路。关键点总结：
> 1. Hexo 负责内容生成 → GitHub 仓库托管 → Cloudflare 全球加速
> 2. 无需服务器，全程免费，访问速度远超 GitHub Pages
> 3. 未来更新只需 git push，Cloudflare 自动部署

<div style="background:rgb(245, 247, 250); padding: 10px; border-left: 3px solid rgb(136, 207, 237); border-radius: 4px; font-size: 15px;">
<span style="font-weight: bold;">本文作者：</span>小东<br>
<span style="font-weight: bold;">博客链接：</span><a href="https://blog.xiaodongblog.xyz/" style="color:rgba(204, 0, 160, 0.48); text-decoration: none;">https://blog.xiaodongblog.xyz/</a><br>
<span style="font-weight: bold;">版权声明：</span>本博客所有文章除特别声明外，均默认采用 <span style="color: rgba(204, 0, 160, 0.48);">CC BY-NC-SA 4.0</span> 许可协议。
</div>



