---
title: 关于Hexo6.0搭建个人博客(主题优化进阶篇)
date: 2020-01-06 10:20:05
tags: hexo
categories: hexo
copyright: true
photos: "https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog1/timg%20%281%29.jpg"
---

# 关于Hexo6.0搭建个人博客(主题优化进阶篇)



# 目录

- 配置博客基本信息
- 配置主题
- 优化主题
  - 上传头像,并设置头像旋转效果
  - 设置个人社交图标链接
  - 设置RSS
  - 设置酷炫动态背景
  - 设置主题语言
  - 设置网站logo
  - 设置左上角或者右上角的fork me on github效果
  - 设置顶部滚动加载条
  - 自定义博客底部显示效果
  - 为首页文章添加阴影边框效果
  - 为首页添加自定义菜单栏标签

# 配置博客基本信息

> 在站点根目录下`_config.yml`中进行基础配置
> 建议下载个文本编辑器打开,这里推荐`Sublime Text`,



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.14460331551001837.png)

set1.png

对应显示效果(显示效果因主题不同而不同,只做描述)





![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.11380374423635242.png)

show.png



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.5111799352238446.png)

show1.png

当然了,你们的主题和我的肯定是不一样的,所以下面就开始教大家挑选自己喜欢的主题
并自定义个人喜好.

# 配置主题

> 挑选主题

你可以点击这里选择你喜欢的[Themes](https://hexo.io/themes/),里面有大量美观的主题



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.39483552872676664.png)

thems.png

> 主题很多,可以慢慢挑选,挑选好了,直接`clone`下来就好了.

我这里以简约著称的`Next`主题为例讲解,本人用的也是这款主题,当然喜欢炫酷一点的小伙伴们可以自行选择,只是优化会略有不同.



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.9894134548979319.png)

next.png

这里有下载主题有两种方式:
1.通过get 命令方式直接安装
2.直接把整个仓库clone到本地,再移动到你的站点目录Themes下

这里主要教大家用命令行方式

> $ cd hexo
> $ git clone https://github.com/theme-next/hexo-theme-next themes/next

进入hexo根目录下,`GitBash`



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.13558235460697166.png)

clone.png

clone成功后,你的Themes文件夹下就会有next主题文件了





![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.27471662652383544.png)

nextfile.png

首先介绍下next文件夹下`_config.yml`文件,以后我们凡是修改主题,都是修改这个文件

然后下一步,我们先应用我们刚才clone下来的主题,看看什么效果

打开blog(`你的博客站点跟目录`)下的_config.yml文件进行设置:
将`theme: landscape`修改为 `theme: next`



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.4336456263234436.png)

config_next.png

然后`hexo g`和`hexo s`一下 ,我们来看一下效果:



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.24971407874585627.png)

nextshow.png

> 好了 ,没错就是这个样子(的确是丑了点,所以需要你自己去优化成你自己想要的样子啦)

当然你也可以直接clone我配置好了的主题[darryrzhong-next](https://github.com/darryrzhong/hexo-theme-next).顺便star一下最好啦.

# 优化主题

到现在我们基础博客算是搭建完成了,下面手把手教大家美化主题,赶快上车吧

带大家优化主题之前,先介绍下next目录结构,了解目录结构能帮助我们更快的优化

```java
.
├── .deploy
├── public
├── scaffolds
├── scripts
├── source
|   ├── _drafts
|   └── _posts
├── themes
├── _config.yml
└── package.json
```

最新的next已经把我们需要的大部分功能都已经集成了,所以我们需要修改的文件只有那么几个

- deploy：执行hexo deploy命令部署到GitHub上的内容目录
- public：执行hexo generate命令，输出的静态网页内容目录
- `source：`站点资源目录,你写的文章,需要的素材等等都是放在这个目录下,包括以后
  你需要新建菜单标面目录也是放在这里.
- drafts：草稿文章
- posts：成功发布的文章目录(你发布的文章都在这里面可以看到 )
- `themes：`主题文件目录
- `_config.yml：`全局配置文件，大多数的设置都在这里
  常用的需要了解的就是这些目录了,其中高亮的地方就是整个next的核心部分了

# 1. 上传头像,并设置头像旋转效果

设置头像:
打开`themes/next/_config.yml`找到`avatar: /images/avatar.gif`;
其中`images`文件在`themes/next\source\`中,将你的头像图片放到images中,一般默认
命名为avatar,记得改下后缀就可以了.
设置旋转效果:
打开`themes\next\source\css\_common\components\sidebar\sidebar-author.styl`,
添加以下注释代码

```css
.site-author-image {
  display: block;
  margin: 0 auto;
  padding: $site-author-image-padding;
  max-width: $site-author-image-width;
  height: $site-author-image-height;
  border: $site-author-image-border-width solid $site-author-image-border-color;

   /* 头像圆形 */
  border-radius: 80px;
  -webkit-border-radius: 80px;
  -moz-border-radius: 80px;
  box-shadow: inset 0 -1px 0 #333sf;

  /* 鼠标经过头像旋转时间 */
  -webkit-transition: -webkit-transform 1.0s ease-out;
  -moz-transition: -moz-transform 1.0s ease-out;
  transition: transform 1.0s ease-out;


}

 img:hover {
  /* 鼠标经过停止头像旋转 
  -webkit-animation-play-state:paused;
  animation-play-state:paused;*/

  /* 鼠标经过头像旋转360度 */
  -webkit-transform: rotateZ(360deg);
  -moz-transform: rotateZ(360deg);
  transform: rotateZ(360deg);
}
```

做完这一步,头像也就设置完成了.





![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.630276789396063.png)

avatar1.png

# 2.设置个人社交图标链接



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.926925378910463.png)

social.png

打开`themes/next/_config.yml`,找到`social`;



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.2177050367074278.png)

social1.png

其中`name`为你图标的名字,你可以去[Font Awesome](http://www.bootcss.com/p/font-awesome/#)中挑选你喜欢的,然后复制名字就可以了
如 icon-user-md 只需要user-md就可以了,next里面已经集成好了.如果next找不到图标的话,图标就会被一个问号所取代.

# 3.设置RSS



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.930278557892666.png)

rss.png

来到你的站点根目录(`比如我的blog/`)下;
执行 GitBash 命令 : `$ npm install --save hexo-generator-feed`安装插件,插件会放在
`node_modules`文件夹里面.

安装好插件后,打开全局配置文件`_config.yml`,在末尾加入以下代码:

```bash
# Extensions
## Plugins: http://hexo.io/plugins/
plugins: hexo-generate-feed
```

然后打开主题配置文件`_config.yml`,找到`rss`;
添加配置:`rss: /atom.xml`;
到这里就行了,重新运行一下 `hexo g`,`hexo s` ,你就会看到效果了.

# 4.设置酷炫动态背景



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.4897253397950685.png)

background.png

新版本的next已经支持canvas-nest了,所以直接添加代码就可以了
打开`next/layout/_layout.swig`文件在</body>之前加上以下代码;

```none
<script type="text/javascript" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js"></script>
{% endif %}
```

打开主题配置文件`_config.yml`, 修改以下代码;

```bash
# Canvas-nest
# Dependencies: https://github.com/theme-next/theme-next-canvas-nest
canvas_nest: true
```

重新运行下就可以了,

# 5.设置主题语言

你的博客首页显示的默认是英文名，而我们想要有一个中文的名字的话,就需要设置下语言显示;
我们可以在next\language文件下的zh-CN（中文）语言包下增加相应的字段,还可以修改其他的字段;

```undefined
title:
  archive: 归档
  category: 分类
  tag: 标签
  schedule: 日程表
menu:
  home: 首页
  archives: 归档
  categories: 分类
  tags: 标签
  about: 关于
  search: 搜索
  schedule: 日程表
  sitemap: 站点地图
  commonweal: 公益 404
sidebar:
  overview: 站点概览
```

改成我们自己想要的显示效果后.在全局配置文件`_config.yml`中重新配置,
`language: zh-CN`,重新运行下服务器记得,只要是全局效果,都需要重新clean一下,在重新运行下,否则看不到效果.

# 6.设置网站logo

效果如下:



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.3999573555979934.png)

logo.png


这个小图标就是你的logo了.打开主题配置文件 `favicon:`

```jsx
favicon:
  small: /images/favicon-16x16-next.png
  medium: /images/favicon-32x32-next.png
  apple_touch_icon: /images/apple-touch-icon-next.png
  safari_pinned_tab: /images/logo.svg
```

可以看到有四种效果,一般我们只需将medium换成我们自己图标路径就行了.
对于图标大小也是有要求的,看实例就知道了,就不做过多的说明了.

# 7.设置左上角或者右上角的fork me on github效果

显示效果如下:



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.19767017941428322.png)

fork me github.png


实现方法如下:当然有很多种颜色和效果,你可以在选择你需要的效果;然后复制你选择效果右侧的代码;

打开`next\layout\_layout.swig`,将刚才复制的代码黏贴到`<div class="headband"></div>`的下面;



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.2794467605250297.png)

github.png

重新运行下,看看效果吧;

# 8. 设置顶部滚动加载条

效果如下:





![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.7428406168283936.png)

bar.png

打开`next\layout\_partials\head`文件，添加以下代码:

```xml
<script src="//cdn.bootcss.com/pace/1.0.2/pace.min.js"></script>
<link href="//cdn.bootcss.com/pace/1.0.2/themes/pink/pace-theme-flash.css" rel="stylesheet">
```



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.6528499710646252.png)

barset.png



以上加载条就可以正常显示了,默认颜色是粉色的,本人觉得粉色的也挺好看就没改了,
当然你可以自定义你想要的颜色,做法如下:
在刚才添加的代码后面再加上以下代码:

```xml
<style>
    .pace .pace-progress {
        background: #1E92FB; /*进度条颜色*/
        height: 3px;
    }
    .pace .pace-progress-inner {
         box-shadow: 0 0 10px #1E92FB, 0 0 5px     #1E92FB; /*阴影颜色*/
    }
    .pace .pace-activity {
        border-top-color: #1E92FB;    /*上边框颜色*/
        border-left-color: #1E92FB;    /*左边框颜色*/
    }
</style>
```

自定义加载条就大功改成了;

# 9.自定义博客底部显示效果



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.6231078477042751.png)

bottom.png

具体实现如下:
1.隐藏网页底部powered By Hexo / 强力驱动
打开`themes/next/layout/_partials/footer.swig`,直接隐藏以下代码即可,建议不要删除
代码如下:

```csharp
<!--{% if theme.footer.powered.enable %}
  <div class="powered-by">{#
  #}{{ __('footer.powered', '<a class="theme-link" target="_blank"' + nofollow + ' href="https://hexo.io">Hexo</a>') }}{% if theme.footer.powered.version %} v{{ hexo_env('version') }}{% endif %}{#
#}</div>
{% endif %}

{% if theme.footer.powered.enable and theme.footer.theme.enable %}
  <span class="post-meta-divider">|</span>
{% endif %}

{% if theme.footer.theme.enable %}
 <div class="theme-info">{#
  #}{{ __('footer.theme') }} &mdash; {#
  #}<a class="theme-link" target="_blank"{{ nofollow }} href="https://github.com/theme-next/hexo-theme-next">{#
    #}NexT.{{ theme.scheme }}{#
  #}</a>{% if theme.footer.theme.version %} v{{ version }}{% endif %}{#
#}</div>
{% endif %}-->
```

2.修改网页底部的桃心

还是在`themes/next/layout/_partials/footer.swig`中，找到字段`with-love`;

```jsx
<span class="with-love" id="animate">
    <i class="fa fa-{{ theme.footer.icon.name }}"></i>
  </span>
```

然后在[图标库](http://www.bootcss.com/p/font-awesome/#)中找到你想要的图标,修改如下:

```jsx
<span class="with-love" id="animate">
    <i class="fa fa-heart"></i>
  </span>
```

其中heart为图标名字,只要icon-后面的就行了;

3.实现网站底部访问量显示
打开主题配置文件`_config.yml`,
修改如下代码

```bash
busuanzi_count:
  enable: true
  total_visitors: true
  total_visitors_icon: user 
  total_views: true
  total_views_icon: eye
  post_views: true
  post_views_icon: eye
```

# 10.为首页文章添加阴影边框效果

打开`next\source\css\_custom\custom.styl`文件,添加以下代码:

```cpp
// 主页文章添加阴影效果
 .post {
   margin-top: 60px;
   margin-bottom: 60px;
   padding: 25px;
   -webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
   -moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);
  }
```

重新运行下服务器,效果如下:





![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.4944870283252676.png)

shandown.png

# 11.为首页添加自定义菜单栏标签

效果如下:





![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.39459319978970986.png)

menu.png

实现如下:
进入站点根目录下,使用`GitBash`新建页面;

> $ hexo new page "music"
> $ hexo new page "photo"
> $ hexo new page "welfare"

这里我新建三个菜单标签,分别是音乐、摄影、福利.





![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.7428281736603286.png)

newPage.png

可以看到,新建页面在source文件对于的文件夹里;

打开主题配置文件`_config.yml`找到字段`menu`,添加对应菜单;



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.17944658597139096.png)

menu1.png

添加完菜单标签项后,效果如下:





![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.10859959584916101.png)

menu2.png

可以看到标签名还是英文的,并且pohto项是空白的,之前已经说过,空白代表这个图标找不到,所以换一个就行了,下面我们来完善一下,将标签名改为中文;

打开`next\language文件下的zh-CN`（中文）语言包,添加菜单标签项;

```undefined
menu:
  home: 首页
  photo: 摄影
  music: 音乐
  welfare: 福利
  archives: 归档
  categories: 分类
```



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog2/0.48439991492151546.png)

menu3.png

名称一定要和我们主题中的名称一致,到这里我们的菜单栏也就完成了.

> 到这里,关于博客的基础搭建也就全部做好了

本文只要介绍了博客外观美化的一些小技巧,使得我们的博客看起来更加的美观,关于博客内文章的美观及评论留言区等的优化、以及使用markdown写下第一篇博文等将会在下一篇高级篇手把手教大家,欢迎关注作者[darryrzhong](http://www.fatyao.com/),更多干货等你来拿哟.

1.[关于Hexo6.0搭建个人博客(基础编)](http://www.fatyao.top/hexoblog1.html)
2.[关于Hexo6.0搭建个人博客(高级篇)](http://www.fatyao.top/hexoblog3.html)
