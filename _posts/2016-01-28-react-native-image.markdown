---
layout: page
title:  "react-native Image使用"
date:   2016-01-28 17:37:00 +05:30
categories: react-native
author: wangxiang
github_repo_username: askday
comments: false
---
在react-native中图片的加载是通过Image组件完成的，Image基本用法如下：

1.加载本地图片

{%highlight  html%}
  <Image  source={require('../image/right_arrow.png')}/>
{%endhighlight%}

2.加载客户端内部已有的图片

ios放在Images.xcassets下的图片

android放在drawable下的图片

{%highlight  html%}
  <Image source={require('image!tip_close_icon')}/>
{%endhighlight%}

3.加载远程url图片

远程图片要指定图片的大小

{%highlight  html%}
<Image
  source = \{\{uri: 'http://ss.bdimg.com/static/superman/img/logo/bd_logo1_31bdc765.png'\}\}
  style = {{width: 200,height: 100}}/>
{%endhighlight%}

4.图片圆角处理

{%highlight  html%}
<Image
  borderRadius={40}
  source = {{uri: userPic}}/>
{%endhighlight%}

5.图片拉升

{%highlight  html%}
  <Image resizeMode='stretch' source={require('image!tip_background_image')} style={styles.wrapper}>
{%endhighlight%}

6.加载图片时设置默认图片

{%highlight  html%}
  <Image
    defaultSource = {{uri:'mine_photo'}}
    source = {{uri: userPic}}/>
{%endhighlight%}
