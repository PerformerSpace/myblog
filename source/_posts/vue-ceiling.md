---
title: Vue项目中的吸顶效果
date: 2020-01-03 16:52:44
tags: vue
categories: Vue
copyright: true
photos: "https://yaoyao-blog.oss-cn-beijing.aliyuncs.com/photo/topPicPre.png"
---

## step1:给选择的元素标签绑定样式,如下:

```
<div ref="showDays" :class="headerFixed?'issFixed':''">
    <ul id="timeline" class="mb-line-b" v-if="movie.showDays">

        <li data-day="2019-01-03"  
        v-for="(item,index) in movie.showDays.dates" 
        class="showDay" 
        :class="{chosen:selectDayIndex==index}"  
        @click="dealClickDay(item,index)">
            {{item.date}}
        </li>
    </ul>
</div>
```



## step2:在script标签中添加如下:



```
定义:headerFixed: 0,
1
```



## step3:在script标签中添加如下:



```
<script>
    mounted() {
    // 监听dom渲染完成
    this.$nextTick(function() {
      // 这里fixedHeaderRoot是吸顶元素的ID
      const header = this.$refs.showDays
      // 这里要得到top的距离和元素自身的高度
      this.offsetTop = header.offsetTop
      this.offsetHeight = header.offsetHeight
      console.log('offsetTop:' + this.offsetTop + ',' + this.offsetHeight)
    })
    // handleScroll为页面滚动的监听回调
    window.addEventListener('scroll', this.handleScroll)
  },
  destroyed() {
    window.removeEventListener('scroll', this.handleScroll)
  },
    methods: {      
    // 搜素框吸顶方法
    handleScroll() {
      // 得到页面滚动的距离
      const scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop
      // 判断页面滚动的距离是否大于吸顶元素的位置
      this.headerFixed = scrollTop > (this.offsetTop - this.offsetHeight * 2)
    },
    },
</script>
```



## step4:在style标签中添加样式如下:



```
<style>
.issFixed{
    position: fixed;
    top:65px;
    left:200px;
    right:0px;
    z-index: 4;
  } 
</style>
```