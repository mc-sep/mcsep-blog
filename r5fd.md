---
title: API构建①——Bing每日一图接口
cover: /img/article/8/th.png
katex: true
categories: PHP
tags:
  - API
  - PHP
abbrlink: 35988
date: 2024-07-31 19:47:00
---

# 前言

众所周知，bing的每日一图都特别beautiful，在我们的blog里也起到了很好的装饰作用。那就有人问了，HOW TO GET IT？今天我就来教你们构建属于自己的bing每日一图API！

# 实践

### Part1. 了解bing每日一图链接规律

就拿今天的做例子：https://cn.bing.com/th?id=OHR.HoodoosBryce_ZH-CN8398575172_1920x1080.jpg

获取到的json内容如下所示：

```json
{
    "images": [
        {
            "startdate": "20240730",
            "fullstartdate": "202407301600",
            "enddate": "20240731",
            "url": "/th?id=OHR.HoodoosBryce_ZH-CN8398575172_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp",
            "urlbase": "/th?id=OHR.HoodoosBryce_ZH-CN8398575172",
            "copyright": "石林，日落点，布莱斯峡谷国家公园，犹他州，美国 (© Tim Fitzharris/Minden Pictures)",
            "copyrightlink": "https://www.bing.com/search?q=%E5%B8%83%E8%8E%B1%E6%96%AF%E5%B3%A1%E8%B0%B7%E5%9B%BD%E5%AE%B6%E5%85%AC%E5%9B%AD&form=hpcapt&mkt=zh-cn",
            "title": "这里有“胡图”吗？",
            "quiz": "/search?q=Bing+homepage+quiz&filters=WQOskey:%22HPQuiz_20240730_HoodoosBryce%22&FORM=HPQUIZ",
            "wp": true,
            "hsh": "1cabac064d004edabc8b0d2e375052ae",
            "drk": 1,
            "top": 1,
            "bot": 1,
            "hs": []
        }
    ],
    "tooltips": {
        "loading": "正在加载...",
        "previous": "上一个图像",
        "next": "下一个图像",
        "walle": "此图片不能下载用作壁纸。",
        "walls": "下载今日美图。仅限用作桌面壁纸。"
    }
}
```

我们就可以猜出其中的规律了：

https://cn.bing.com + 上面的"url"，构成了该链接。

### Part2. 构建API

我们使用**GET**方法构建我们的API，名称为`bing.php`，也可以从 https://mc-sep.js.cool/api/bing.php 下载！

{% note warning disabled %}
注意：实操里要养成打注释的习惯！
{% endnote %}

1. 判断是否是随机调用（我们的API可以指定日期）
```php
<?php
if ($_GET['rand']==='true') {
  $gettime = rand(-1,7);
```

2. 如果不是，则指定日期

```php
else{
  $gettimebase = $_GET['day'];
  if (empty($gettimebase)) {
    $gettime = 0;
  }else{
    $gettime = $gettimebase;
  }
}
```

3. 获取Bing Json详细信息，转换为PHP数组，再提取基础URL

```php
$json_string = file_get_contents('https://www.bing.com/HPImageArchive.aspx?format=js&idx='.$gettime.'&n=1');
$data = json_decode($json_string);
$imgurlbase = "https://www.bing.com".$data->{"images"}[0]->{"urlbase"};
```

4. 判断是否制定了图片大小，否则默认1920 * 1080

```php
if (empty($imgsizebase)){
  $imgsize = "1920x1080";
}else{
  $imgsize = $imgsizebase;
}
```

5. 建立完整URL

```php
$imgurl = $imgurlbase."_".$imgsize.".jpg";
```

6. 获取其他信息并判断是否需要

```php
$imgtime = $data->{"images"}[0]->{"startdate"};
$imgtitle = $data->{"images"}[0]->{"copyright"};
$imglink = $data->{"images"}[0]->{"copyrightlink"};
if ($_GET['info']==='true') {
  echo "{title:".$imgtitle.",url:".$imgurl.",link:".$imglink.",time:".$imgtime."}";
}
```

7. 不是则直接返回

```php
else{
  header("Location: $imgurl");
}
```

8. 完整代码

```php
<?php
if ($_GET['rand']==='true') {
  $gettime = rand(-1,7);
}else{
  $gettimebase = $_GET['day'];
  if (empty($gettimebase)) {
    $gettime = 0;
  }else{
    $gettime = $gettimebase;
  }
}
$json_string = file_get_contents('https://www.bing.com/HPImageArchive.aspx?format=js&idx='.$gettime.'&n=1');
$data = json_decode($json_string);
$imgurlbase = "https://www.bing.com".$data->{"images"}[0]->{"urlbase"};
$imgsizebase = $_GET['size'];
if (empty($imgsizebase)){
  $imgsize = "1920x1080";
}else{
  $imgsize = $imgsizebase;
}
$imgurl = $imgurlbase."_".$imgsize.".jpg";
$imgtime = $data->{"images"}[0]->{"startdate"};
$imgtitle = $data->{"images"}[0]->{"copyright"};
$imglink = $data->{"images"}[0]->{"copyrightlink"};
if ($_GET['info']==='true') {
  echo "{title:".$imgtitle.",url:".$imgurl.",link:".$imglink.",time:".$imgtime."}";
}else{
  header("Location: $imgurl");
}
```
   
# 总结

没啥好总结的。可以去参考一下API接口：

- https://api.kdcc.cn
- https://api.isoyu.com/bing_images.php
- http://api.muvip.cn//api/bing/index.php?rand=false&day=0&size=1920x1080&info=false
- http://img.xjh.me/random_img.php
- https://source.unsplash.com/random
- https://api.dujin.org/bing/1366.php
- https://picsum.photos/

版权所有©MC-Sep 2024.