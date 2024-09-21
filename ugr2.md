---
title: 如何让你的博客更丰富？hexo常见插件推荐
cover: /img/article/12/th.png
katex: true
categories: Hexo
tags:
  - Hexo插件
  - Hexo
abbrlink: 51096
date: 2024-08-25 13:24:00
---

# 前言

如果你的 hexo 博客只是普通的搭建完成，那么也会缺少一些新意。下面是一些实用的hexo插件。

# hexo插件集

### hexo-abbrlink

1. 安装：

```bash
npm install hexo-abbrlink --save
```

2. 当你hexo g时，这个插件会对未生成abbrlink的文章中的front matter里添加这个东西（据说此种格式有利于SEO优化）：

```frontmatter
abbrlink: *****（随机数字）
```

3. 也可以自定义，尽量使用字母和数字，空格用`-`替代，提升链接可读性。

```frontmatter
abbrlink: 我的文章-my-post-*****
```

### hexo-encrypt

1. 安装

```bash
npm install --save hexo-blog-encrypt
```

2. 在front matter里输入`password: 你的密码`，如：

```frontmatter
password: hello
```

3. 那么在hexo中就是这样的画面：

![加密后](/img/article/12/d068699b430414e617c92223c6e4ffc049da653fc65e572010c96bb320746b6f.png)

4. 可以添加更多，如：

```frontmatter
---
title: Hello World
date: 2016-03-30 21:12:21
password: 114514
abstract: 有东西被加密了, 请输入密码查看. //系统信息
message: 您好, 这里需要密码. //展示的信息
wrong_pass_message: 抱歉, 这个密码看着不太对, 请再试试. //密码错误
wrong_hash_message: 抱歉, 这个文章不能被校验, 不过您还是能看看解密后的内容. //hash解密错误
---
 
```

或者在`_config.yml`里添加这些东西：

```yaml
# Security
encrypt: # hexo-blog-encrypt
  abstract: 有东西被加密了, 请输入密码查看.
  message: 您好, 这里需要密码.
  theme: xray
  wrong_pass_message: 抱歉, 这个密码看着不太对, 请再试试.
  wrong_hash_message: 抱歉, 这个文章不能被校验, 不过您还是能看看解密后的内容.
 
```

### hexo-generator

1. 安装（先第一个后第二个）

```bash
npm uninstall hexo-generator-index --save
```

```bash
npm install hexo-generator-index-pin-top --save
```

2. 在front matter里添加`top: ***`数字越大文章越靠前

```frontmatter
top: 114514
```

### hexo-hide-posts

1. 安装

```bash
npm install hexo-hide-posts --save
```

2. 默认 `hidden: true` 用于隐藏，可以修改成`false`

### hexo-generator-feed

1. 安装

```bash
npm install hexo-generator-feed --save
```

2. 当你hexo d时会生成用于rss订阅，默认只获取文章摘要或者前140个字符

### hexo-filter-nofollow

1. 安装

```bash
npm i hexo-filter-nofollow --save
```

2.  可排除自定义域名。

### hexo-submit-urls-to-search-engine

1. 安装

```bash
npm install --save hexo-submit-urls-to-search-engine
```

2. hexo d时自动提交链接到搜索引擎。提交效果不错，但百度放出页面的速度依旧感人，谷歌、必应、雅虎的收录速度很快。

### markdown-it-ruby 与 hexo-pinyin-ruby-marks

1. 安装

```bash
npm install markdown-it-ruby --save
```

```bash
npm install hexo-pinyin-ruby-marks
```

2. 在`_config.yml`中输入以下内容：

```yaml
markdown:
  plugins:
    - markdown-it-ruby
```

3. 在文章中输入以下内容：

```
{% pinyin 初音未来|chu1 yin1 wei4 lai2 %} 
```

4. 即可看到文字上的拼音注音效果

### hexo-pangu

1. 安装

```bash
npm install hexo-pangu
```

2. 送给所有中英混输必须加空格的强迫症患者。（但愿跟其它插件不存在冲突）

### hexo-broken-links-checker

1. 安装

```bash
npm install hexo-broken-links-checker --save-dev
```

2. 检查

```bash
npx hexo check-links
```

3. 等待一会就会出现：

```bash
WARN  The following links are broken:
   - https://
```

### hexo-tag-qrcode

1. 安装

```bash
npm i hexo-tag-qrcode
```

2. 在文章中输入：

```
{% qrcode "自定义内容" %}
```

# 后记

文章中的插件可以评论提示我你喜欢的插件，我可能会采纳！

版权所有©MC-Sep 2024.