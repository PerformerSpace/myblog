---
title: 关于Hexo6.0搭建个人博客(主题优化高级篇)
date: 2020-01-06 10:20:13
tags: hexo
categories: hexo
copyright: true
photos: "https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog3/timg%20%285%29.jpg"
---

# 关于Hexo6.0搭建个人博客(主题优化高级篇)



> 本篇博文将继续带大家深入文章效果优化,教你打造炫酷的个人博客站点.

阅读本文前建议先行阅读本人另外两篇遍基础博文
1.[关于Hexo6.0搭建个人博客(基础篇)](http://www.fatyao.top/hexoblog1.html)
2.[关于Hexo6.0搭建个人博客(进阶篇)](http://www.fatyao.top/hexoblog2.html)

# 目录

- 优化博客文章样式
  - 修改文章内链接文本样式
  - 修改文章底部的的标签样式
  - 在每篇文章末尾统一添加“本文结束”标记
  - 实现文章字数统计和阅读预估时间
  - 在文章底部增加版权信息
  - 文章加密访问
  - 将博文置顶
  - 开启文章底部打赏功能
  - 开启留言评论功能

# 1.修改文章内链接文本样式

打开文件`themes\next\source\css\_common\components\post\post.styl`，在末尾添加以下css样式:

```csharp
// 文章内链接文本样式
.post-body p a{
  color: #0593d3;
  border-bottom: none;
  border-bottom: 1px solid #0593d3;
  &:hover {
    color: #fc6423;
    border-bottom: none;
    border-bottom: 1px solid #fc6423;
  }
}
```

其中颜色可以自定义,在这里选中状态为橙色,链接样式为蓝色.效果如下:


![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog3/0.7328885965600911.png)

style.png

# 2. 修改文章底部的的标签样式

打开模板文件`/themes/next/layout/_macro/post.swig`，找到`rel="tag">#`字段，
将`# 换成<i class="fa fa-tag"></i>`,其中tag是你选择标签图标的名字,也是可以自定义的,
如下:

```
<a href="{{ url_for(tag.path) }}" rel="tag"> <i class="fa fa-tag"></i> {{ tag.name }}</a>
```



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog3/0.5647113882547175.png)

biaoq.png

# 3. 在每篇文章末尾统一添加“本文结束”标记

效果如下:


![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog3/0.8960303179700639.png)

end.png

实现方法如下:

1. 在`\themes\next\layout\_macro`下新建 passage-end-tag.swig 文件,并添加以下代码：

```xml
<div>
    {% if not is_index %}
        <div style="text-align:center;color: #ccc;font-size:14px;">-------------本文结束<i class="fa fa-heart"></i>感谢阅读-------------</div>
    {% endif %}
</div>
```

1. 打开`\themes\next\layout\_macro\post.swig`文件，在post-body 之后， post-footer （post-footer之前有两个DIV）之前添加如下代码：

```php
<div>
  {% if not is_index %}
    {% include 'passage-end-tag.swig' %}
  {% endif %}
</div>
```



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog3/0.7906940192875778.png)

footer.png

1. 打开主题配置文件`_config.yml`在末尾添加以下字段：

```bash
# 文章末尾添加“本文结束”标记
passage_end_tag:
  enabled: true
```

到此,就大功告成了.

# 4.实现文章字数统计和阅读预估时间

效果如下:





![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog3/0.9235627112257534.png)

count.png

1.在站点根目录下使用`GitBash`命令安装 `hexo-wordcoun`t插件:

> $ npm install hexo-wordcount --save

2.在全局配置文件`_config.yml`中激活插件:

```bash
ymbols_count_time:
    symbols: true
    time: true
    total_symbols: true
    total_time: true
```

3.在主题的配置文件`_config.yml`中进行如下配置:

```symbols_count_time
  time_minutes: true
  separated_meta: true
  item_text_post: true
  item_text_total: true
  awl: 4
  wpm: 275
```

到此,我们就实现了文章字数统计和预估时间的显示功能

# 5. 在文章底部增加版权信息

效果如下:





![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog3/0.8078661482425074.png)

show1.png

1.在`next/layout/_macro/`下新建文件`my-copyright.swig`：
添加以下代码:

```jsx
  {% if page.copyright %}
<div class="my_post_copyright">
  <script src="//cdn.bootcss.com/clipboard.js/1.5.10/clipboard.min.js"></script>

  <!-- JS库 sweetalert 可修改路径 -->
  <script src="https://cdn.bootcss.com/jquery/2.0.0/jquery.min.js"></script>
  <script src="https://unpkg.com/sweetalert/dist/sweetalert.min.js"></script>
  <p><span>本文标题:</span><a href="{{ url_for(page.path) }}">{{ page.title }}</a></p>
  <p><span>文章作者:</span><a href="/" title="访问 {{ theme.author }} 的个人博客">{{ theme.author }}</a></p>
  <p><span>发布时间:</span>{{ page.date.format("YYYY年MM月DD日 - HH:MM") }}</p>
  <p><span>最后更新:</span>{{ page.updated.format("YYYY年MM月DD日 - HH:MM") }}</p>
  <p><span>原始链接:</span><a href="{{ url_for(page.path) }}" title="{{ page.title }}">{{ page.permalink }}</a>
    <span class="copy-path"  title="点击复制文章链接"><i class="fa fa-clipboard" data-clipboard-text="{{ page.permalink }}"  aria-label="复制成功！"></i></span>
  </p>
  <p><span>许可协议:</span><i class="fa fa-creative-commons"></i> <a rel="license" href="https://creativecommons.org/licenses/by-nc-nd/4.0/" target="_blank" title="Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)">署名-非商业性使用-禁止演绎 4.0 国际</a> 转载请保留原文链接及作者。</p>  
</div>
<script> 
    var clipboard = new Clipboard('.fa-clipboard');
      $(".fa-clipboard").click(function(){
      clipboard.on('success', function(){
        swal({   
          title: "",   
          text: '复制成功',
          icon: "success", 
          showConfirmButton: true
          });
        });
    });  
</script>
{% endif %}
```

2.在`next/source/css/_common/components/post/`下新建文件`my-post-copyright.styl`：
添加以下代码:

```objectivec
.my_post_copyright {
  width: 85%;
  max-width: 45em;
  margin: 2.8em auto 0;
  padding: 0.5em 1.0em;
  border: 1px solid #d3d3d3;
  font-size: 0.93rem;
  line-height: 1.6em;
  word-break: break-all;
  background: rgba(255,255,255,0.4);
}
.my_post_copyright p{margin:0;}
.my_post_copyright span {
  display: inline-block;
  width: 5.2em;
  color: #b5b5b5;
  font-weight: bold;
}
.my_post_copyright .raw {
  margin-left: 1em;
  width: 5em;
}
.my_post_copyright a {
  color: #808080;
  border-bottom:0;
}
.my_post_copyright a:hover {
  color: #a3d2a3;
  text-decoration: underline;
}
.my_post_copyright:hover .fa-clipboard {
  color: #000;
}
.my_post_copyright .post-url:hover {
  font-weight: normal;
}
.my_post_copyright .copy-path {
  margin-left: 1em;
  width: 1em;
  +mobile(){display:none;}
}
.my_post_copyright .copy-path:hover {
  color: #808080;
  cursor: pointer;
}
```

1. 打开`next/layout/_macro/post.swig`文件,添加以下代码:

```php
<div>
      {% if not is_index %}
        {% include 'my-copyright.swig' %}
      {% endif %}
</div>
```

添加为止如下:找到`wechat-subscriber`字段,在其字段之前加入上述代码



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog3/0.44975897006101295.png)

copyright.png

4.打开`next/source/css/_common/components/post/post.styl`文件，在末尾增加代码：

> ```
> @import "my-post-copyright"
> ```

到此版权信息说明也就成功了.
说明一下,想要在文章底部显示版权信息,需要在文章头部`copyright: true`才可以正常显示

# 6.文章加密访问

打开`next\layout\_partials\head\head.swig`文件,插入以下代码:

```xml
<script>
    (function(){
        if('{{ page.password }}'){
            if (prompt('请输入文章密码') !== '{{ page.password }}'){
                alert('密码错误！');
                history.back();
            }
        }
    })();
</script>
```

插入位置如下:



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog3/0.08347699981863865.png)

base64.png


这样如果你的文章需要加密处理的话,就可以在头部写上;如:

效果如下:





![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog3/0.8306667760013073.png)

password.png

到此,文章解密功能就完成了.

# 7.将博文置顶

打开文件：`node_modules/hexo-generator-index/lib/generator.js`,将以下代码替换原来的;

```jsx
'use strict';
var pagination = require('hexo-pagination');
module.exports = function(locals){
  var config = this.config;
  var posts = locals.posts;
    posts.data = posts.data.sort(function(a, b) {
        if(a.top && b.top) { // 两篇文章top都有定义
            if(a.top == b.top) return b.date - a.date; // 若top值一样则按照文章日期降序排
            else return b.top - a.top; // 否则按照top值降序排
        }
        else if(a.top && !b.top) { // 以下是只有一篇文章top有定义，那么将有top的排在前面（这里用异或操作居然不行233）
            return -1;
        }
        else if(!a.top && b.top) {
            return 1;
        }
        else return b.date - a.date; // 都没定义按照文章日期降序排
    });
  var paginationDir = config.pagination_dir || 'page';
  return pagination('', posts, {
    perPage: config.index_generator.per_page,
    layout: ['index', 'archive'],
    format: paginationDir + '/%d/',
    data: {
      __index: true
    }
  });
};
```

在文章中添加 top 值，数值越大文章越靠前，

```title
tag: hexo 
copyright: true
password: 123456
top: 150
---
```

# 8.开启文章底部打赏功能

效果如下:





![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog3/0.5246145489610459.png)

Alipay.png

1.准备支付宝和微信二维码
2.在主题配置文件`_config.yml`中配置图片及信息

```tsx
# Reward
reward_comment: 坚持原创技术分享，您的支持将鼓励我继续创作！
wechatpay: /images/wechatpay.jpg
alipay: /images/alipay.jpg
#bitcoin: /images/bitcoin.png
```

将对应得二维码图片路径换成自己的就可以了.
3.设置打赏字体不闪动
打开`next\source\css\_common\components\post\post-reward.styl`文件,注释以下代码即可:

```bash
/*注释文字闪动
#wechat:hover p{
    animation: roll 0.1s infinite linear;
    -webkit-animation: roll 0.1s infinite linear;
    -moz-animation: roll 0.1s infinite linear;
}
#alipay:hover p{
    animation: roll 0.1s infinite linear;
    -webkit-animation: roll 0.1s infinite linear;
    -moz-animation: roll 0.1s infinite linear;
}
*/
```

ok,这样就完美开启了我们的打赏功能了.

# 9.开启留言评论功能

效果如下:





![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog3/0.680280121501387.png)

chart.png

> 这里说明下,本人所使用的是搜狐的畅言开放评论,而且在本地服务器上跑时不能留言,所以只是教大家集成评论功能,之后会教大家如何使用它接收留言提醒和回复评论等.

1.获取畅言评论的APP ID 和APP KEY
首先注册账号,登录成功后进入后台总览页面底部会有我们需要的`APP ID 和APP KEY`
如下:



![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog3/0.2131928486638761.png)

id_key.png

2.配置畅言评论
打开 主题配置文件`_config.yml`,找到`changyan`字段;将刚才的id和key填入

```bash
# changyan
changyan:
  enable: true
  appid: *********
  appkey: *************************
```

next本身已经集成了畅言,所以到这里我们算是成功开启了评论留言功能了.
重新运行下,看看效果吧;





![img](https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/hexoblog3/0.050999093483176994.png)

changyan.png



由于使用畅言需要站点备案,很多朋友就放弃了,其实备案很容易,在下一篇博文我也会教大家如何破解的.
对的 就是这样子的,看到这样子就证明我们成功了.

> 到此,我们的文章优化基本算是完成了,到目前为止我一直都是教大家在本地打造个人博客,也就是只有自己电脑上才能访问,我想很多朋友肯定已经迫不及待的想要将博客上线运营了,所以我将在下一篇终极篇教大家将站点上线托管至GitHub,SEO优化、以及发表第一篇博文等,让你开始真正拥有一个属于自己的个人站点,欢迎关注[darryrzhong](http://fatyao.top/),更多干货等你来拿哟!
