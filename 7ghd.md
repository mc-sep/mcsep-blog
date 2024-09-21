---
title: 【Hexo】 Waline--评论系统的添加(1)
cover: /img/article/2/th.png
categories: Hexo
recommend: true
tags:
  - Hexo
  - Waline
ai_text: 本文介绍 Waline 评论系统，提供教程和说明。它相对轻量、高性能、安全，支持 Markdown 语法。文章还介绍了配置步骤，包括主题配置、评论配置以及与 Leancloud、Vercel 或 Deta 的设置。
desc: 本文介绍 Waline 评论系统，提供教程和说明。它相对轻量、高性能、安全，支持 Markdown 语法。文章还介绍了配置步骤，包括主题配置、评论配置以及与 Leancloud、Vercel 或 Deta 的设置。
abbrlink: 56565
date: 2024-06-16 10:18:00
---

{% note info simple %}预计完成时间43分钟，请放心食用！{% endnote %}



# 前言：

众所周知，一个博客是不能没有评论的！

`2024.5.2`：上线了`waline`评论系统

`2024.5.9`：关闭了匿名评论功能

`2024.5.14`：启动了双评论，另一个是`twikoo`

`2024.5.18`：关闭了双评论

`2024.6.14`：重新启动了双评论

但是很多人不知道用什么评论......

所以，今天还是老样子出教程！

# 实践

### Part1:了解主流的评论系统

| 评论系统 | [Valine](https://valine.js.org/) | [Waline](https://waline.js.org/) | [Twikoo](https://twikoo.js.org/) | [Artalk](https://artalk.js.org/) | [Gitalk](https://github.com/gitalk/gitalk) | [Giscus](https://giscus.app/zh-CN) | [畅言](https://changyan.kuaizhan.com/) | [Remark42](https://remark42.com) |
| :----------: | :----------: | :----------: | :----------: | :----------: | :----------: | :----------: | :----------: | :----------: |
| 推荐指数 | ✔✔ | ✔✔✔✔ | ✔✔✔✔✔ | ✔✔✔ | ✔✔ | ✔✔ | ✔✔✔ | ✔✔ |
| 是否可以白嫖 | ❌ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ❌ |
| 说明 | 近期被爆出会泄露用户IP | 方便，免费 | 方便，免费 | 方便，要服务器 | 访问慢 | 访问慢 | 要登录，功能全 | 访问慢 |

这里用Waline说明。

### Part2:主题配置文件

1. 修改`主题配置文件`：
```bash
# 评论
# comment
# getting start: https://solitude.js.org/comments/comment
comment:
  use: waline # use
  commentBarrage: true # 热评开关 / Hot comment switch
  lazyload: true # 懒加载
  count: true # 评论数展示
  pv: false # 是否使用評論統計頁面訪問
  avatar: https://cravatar.cn # Gravatar link
  newest_comment:
    enable: ture
    storage: .5 # 缓存时间 1: 1天 / .5 : 半天 / Cache time 1: 1 day .5 : half a day
```

如果使用双评论，修改：
```bash
use: 你的评论1, 你的评论2 # waline, twikoo, valine, artalk
```

随后修改Waline的配置：

```bash
# waline settings
waline: # https://waline.js.org/
  envId:  # 你的部署环境
  pageview: false # 懒加载
  option: # 更多设置，可以去官网找
```

# Part3:配置评论

教程出自：{% link 官网,Waline,https://waline.js.org/ %}

特色：
- 轻量级与高性能：Waline优化了代码结构，确保快速加载和响应，不增加网站负担。
- 安全性：系统内置了多种安全机制，如垃圾邮件过滤（Akismet支持）、IP限制等，保护您的网站免受恶意评论的侵扰。
- Markdown 支持：全面支持Markdown语法，让用户可以方便地插入代码、链接、表情等元素，让评论更加生动。
- 多平台兼容：不仅有前端组件@waline/client，还有多种服务器端解决方案，如Vercel、Deta、CloudBase等，满足不同开发需求。

配置：

## 准备——配置**Leancloud**

1. 进入{% btn 'https://console.leancloud.app/register',Leancloud国际版,st-lightbulb-line,outline %} 进行注册并进入控制台：

![](/img/article/2/14bf60d9d7fd5964ccee88f86c9139cb.webp)

{% note warning simple %}

如果你正在使用 Leancloud 国内版 (leancloud.cn)，我们推荐你切换到国际版 (leancloud.app)。否则，你需要为应用额外绑定已备案的域名，同时购买独立 IP 并完成备案接入:

登录国内版并进入需要使用的应用
选择 设置 > 域名绑定 > API 访问域名 > 绑定新域名 > 输入域名 > 确定。
按照页面上的提示按要求在 DNS 上完成 CNAME 解析。
购买独立 IP 并提交工单完成备案接入。(独立 IP 目前价格为 ￥ 50/个/月)

![](img/article/2/leancloud-3-CT_lZM0A.png)

{% endnote %}

2. 点击左上角**创建应用**，选择**开发板**（免费）

![](/img/article/2/e26c2a7f16d7a7164cb0ecb98c5b390e.webp)

3. 进入应用，选择左下角的 设置 > 应用 Key。你可以看到你的 APP ID,APP Key 和 Master Key，记录它们以便后续使用。

![](/img/article/2/58676c1bb4fd2f35fb23a213fd21dac3.webp)


## 方式1——Vercel

由于大陆域名无法访问，没有域名的小伙伴们可以试试这个：

{% link Willin Wang Free Domains,免费域名,https://domain.willin.wang/ %}

点击一键部署：

[![](/img/article/2/default.svg)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fwalinejs%2Fwaline%2Ftree%2Fmain%2Fexample)

1. 点击上面的按钮，输入你喜欢的名字，点击Create，等待部署：

![](/img/article/2/0574cea0bdc82870997e823195e5348b.webp)

2. 之后满屏的烟花，点击**Continue to Dashboard**

![](/img/article/2/2d8c921741af863b046ebbb8d7b7cfe5.webp)

3. 点击顶部的 Settings - Environment Variables 进入环境变量配置页，并配置三个环境变量 LEAN_ID, LEAN_KEY 和 LEAN_MASTER_KEY 。它们的值分别对应上一步在 LeanCloud 中获得的 APP ID, APP KEY, Master Key。

![](/img/article/2/c6f4d6bced949fdb7ec9bdef576dab88.webp)

4. 回到开头的免费域名，注册一下并关注作者，可以获得5免费域名，点击**创建**：

![](/img/article/2/85bae92761dd2705683b4185a7a26f6d.webp)

5. 输入名字，按如下填写：

![](/img/article/2/5bf63e5dfd90864d24ada804360e52ed.webp)

6. 你就等待审核，预计1小时~2天

![](/img/article/2/496a8213fcfe1b3eac0793bfc0ea0077.webp)

7. 审核完成后会显示**编辑**按钮，回到Vercel，点击**Domains**：

![](/img/article/2/1bbc317d1b20d9b5532b2850505292c9.webp)

8. 填写你申请的域名，点击**Add**：

![](/img/article/2/a438e0447a386031df099a74e7f39961.webp)

9. 等待他解析，然会就可以访问了！

![](/img/article/2/2244070e801aa2177e1925005fb5b874.webp)

## 方式2——Deta

Deta 是一个可免费使用的 Serverless 部署平台。我们可以快速的将 Waline 部署到 Deta 平台上。

点击按钮快速部署：

[![](/img/article/2/dark.svg)](https://deta.space/discovery/@lizheming/waline)

1. 点击上方按钮，点击**Install on Space**

![](/img/article/2/deta-b3wwcCqL.png)

2. 稍等片刻即可在 https://deta.space 首页上看到应用，点击后会打开部署后的网站地址。将其填入前端脚本的 serverURL 配置中，即可完成全部配置。在该网址后增加 /ui 可以进入后台管理界面。

![](/img/article/2/deta-1-BsA0eTbz.png)

3. 修改环境变量：

在应用程序列表 Waline App 下点击 ... - Settings - Configuration 可以对所有的环境变量进行配置。配置完成后点击底部的 Save Changes 保存即可。

{% note danger disabled %}
不建议修改已经填写的变量！
{% endnote %}

![](/img/article/2/deta-4-84hJHkZt.png)

## 不推荐的其他方式：

1. CloudBase的免费数据库太低，每天只可以请求500次......
2. Netlify在中国已经把注册ban了......
3. Railway的免费额度不足以一个月连续使用......
4. Zeabur需要购买（5$每月），否则计划会在5天内被结束......
5. 其他的都复杂且没用......

想“尝试”的可以自行去官网！

# 总结

Waline轻量方便，不亚于Twikoo，博客评论选择它也挺不错的！

版权所有©MC-Sep。