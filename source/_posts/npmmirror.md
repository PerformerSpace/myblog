---
title: npm镜像源管理
date: 2020-01-06 10:02:49
tags: [npm,镜像源管理]
categories: npm
copyright: true
---

# npm镜像源管理



此文列出常见npm命令，大家共勉。
1、查看npm源地址
`npm config list`

结果:
`metrics-registry = "http://registry.npm.taobao.org/"`

2、修改registry地址，比如修改为淘宝镜像源。
`npm set registry https://registry.npm.taobao.org/`
如果有一天你肉身FQ到国外，用不上了，用rm命令删掉它
`npm config rm registry`

3、nrm是专门用来管理和快速切换私人配置的registry
建议全局安装
`npm install nrm -g --save`
用nrm ls命令查看默认配置，带*号即为当前使用的配置
`nrm ls`

```cpp
 npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
* taobao - https://registry.npm.taobao.org/
  nj ----- https://registry.nodejitsu.com/
  rednpm - http://registry.mirror.cqupt.edu.cn/
  npmMirror  https://skimdb.npmjs.com/registry/
  edunpm - http://registry.enpmjs.org/
```

也可以直接输入以下命令查看当前使用的是哪个源
`nrm current`
切换源
`nrm use cnpm`
用nrm add 命令添加公司私有npm源，如[http://registry.npm.360.org(](https://links.jianshu.com/go?to=http%3A%2F%2Fregistry.npm.360.org()随便写的)，起个别名叫qihoo
`nrm add qihoo http://registry.npm.360.org`

测试下速度
`nrm test npm`
输出`npm ---- 790ms`

最后，如果你被公司开除了，怒删公司npm源配置
`nrm del qihoo`

#### 4、更新`Node`版本和`npm`版本

##### 清除node的cache（清除node的缓存，这个看情况而定，不是必须的）

```undefined
sudo npm cache clean -f 
```

##### 安装"n"版本管理工具，管理node（没有错，就是n）

```undefined
sudo npm install -g n 
```

##### 更新node版本

```css
sudo npm install npm@latest -g 
```

##### 再查一遍本机当前Node和npm的版本吧

```undefined
node -v 
npm -v 
```

参考: [https://www.cnblogs.com/wangmeijian/p/7072053.html](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.cnblogs.com%2Fwangmeijian%2Fp%2F7072053.html)