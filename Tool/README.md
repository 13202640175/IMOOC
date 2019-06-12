# 前言

由于这会是一篇很长的课程学习总结，为了优化大家的阅读体验，强烈建议安装Chrome浏览器的插件——GayHub。[下载安装地址](https://github.com/jawil/GayHub)<br>


# 1、JadePro => [带你学习Jade模板引擎](https://www.imooc.com/learn/259)
一篇简单易懂的学习教程，除了后面两个章节代码无法跑起来，其他的都是毫无问题。通过这门课程，相信以后接触新的模板引擎也是能够快速的上手，下面请看我的一些课程总结。<br>

## 什么是模板引擎
将动静部分糅合的一种实现机制或者技术。

## 什么是Jade
jade是使用JavaScript实现，可供nodejs使用的高性能模板引擎（性能高不高，有些争议。姑且称之为高性能吧！）。模板引擎有很多，主要使用比较广泛的是jade和ejs，NodeJs项目默认使用jade作为模板引擎

## Jade特点
- 超强的可读性
- 灵活易用的缩进
- 块扩展
- 代码默认经过编码处理，以增强安全性
- 编译及运行时的上下文错误报告
- 命令行编译支持
- HTML5模式
- 可选的内存缓存
- 联合动态的静态标记类
- 利用过滤器解析树的处理

## jade的缺点
- 可移植性差
- 调试困难
- 性能不是非常出色

## 使用命令行进行编译
首先需要安装好Node环境以及jade(npm install -g jade)，然后用命令分别执行下面的代码查看结果
```html
doctype html
html
  head
    meta(charset='utf-8')
    title imooc jade study
    style.
      body{color:#ccc;}
  body
    h1.title imooc jade study
    h1#title imooc jade study
    h1(id='title',class='title') imooc jade study
    a(href='https://imooc.com') link
    input(name='coure',type='text',value='')
    p
     |大段文本1
     |大段文本2
     |大段文本3
     strong 大段文本4
     |大段文本5
    h1 单行注释
    // h1 我被注释啦
    h1 非缓冲注释
    //- h2 什么意思也不是很懂
    div
      h3
        small 我是嵌套
    script.
      var name = '脚本书写方式'
```
上面的示例代码中，包含了很多知识点，大家可以仔细琢磨琢磨一番。然后在node环境中执行如下命令
- jade index.jade表示的是压缩编译；
- jade -P index.jade表示的是格式化编译；
- jade -P -w index.jade表示的是格式编译并且对源文件进行实时监控。

## jade变量声明和传递
变量声明既可以在文件中进行声明，也能够在命令行中进行声明，还可以通过json文件来声明各种变量数据，并且变量还具有方法。下面我们先看变量在文件中声明
```html
doctype html
html
  head
    meta(charset='utf-8')
    - var course = 'jade'
    title #{course}
  body
    h2 #{course.toUpperCase()}
```

再来看看在命令行中进行声明，执行的时候输入命令：jade index2-7.jade -P --obj '{"course":"jadeeedd"}'
```html
doctype html
html
  head
    meta(charset='utf-8')
    title #{course}
  body
    h2 #{course.toUpperCase()}
```
要是变量既在文件中声明了，又在命令行中声明了，那么会以文件中的变量为主。文件变量的优先级较高，最后我们来看一看通过json格式来声明变量
```html
// json文件
{
  "course": "newblity"
}

// 输入命令：jade index2-7.jade -P -O imooc.json
doctype html
html
  head
    meta(charset='utf-8')
    title #{course}
  body
    h2 #{course.toUpperCase()}
```

## 安全转义与非转义
在jade模板编译的时候可以把一些特殊符号进行转义，下面请看代码
```html
doctype html
html
  head
    meta(charset='utf-8')   
    title 转义
  body
    h2 转义
    - var data = 'text'
    - var htmlData = '<script>console.log("<和>有可能会被转义")</script>'
    p #{data}
    p #{htmlData}
    p !{htmlData}
    p= data
    p= htmlData
    p!= htmlData
    p \#{htmlData}
    p \!{htmlData}
```
使用命令在node环境中查看结果：jade -P index2-8.jade

## Jade中的遍历
不啰嗦啦，直接看代码
```html
doctype html
html
  head
    meta(charset='utf-8')   
    title 转义
  body
    h2 流程
    h3 for
    - var imooc = {course: 'jade',level: 'high'}
    - for (var k in imooc)
      p= imooc[k]
    h3 each
    - each value, key in imooc
      p #{key}: #{value}
    each value, key in imooc
      p #{key}: #{value}
    h3 遍历数组
    - var courses = ['node','vue','react']
    each item in courses
      p= item
    h3 嵌套循环
    - var sections = [{id: 1, items:['a', 'b']}, {id: 2, items:['c', 'd']}]
    dl
      each section in sections
        dt= section.id
        each item in section.items
          dd= item
    dl
      each section in sections.length > 0 ? sections:[{id:0,items:['none']}]
        dt= section.id
        each item in section.items
          dd= item
    h3 while
    - var n = 0
    ul
      while n<4
        li= n++
```

## jade中的if else
看源码就能明白些大概了
```html
doctype html
html
  head
    meta(charset='utf-8')   
    title 转义
  body
    h2 流程
    h3 if else
    - var isImooc = true
    - var lessons = ['jade','node']
    if lessons
      if lessons.length >= 2
        p more than 2: #{lessons.join(',')}
      else if lessons.length > 1
        p more than 1: #{lessons.join(',')}
      else
        p no lesson
    else
     p no lesson
   h3 unless
   unless !isImooc
     p #{lessons.length}
   h3 case
   - var name = 'jade'
   case name
     when 'java'
       p hi java
     when 'node'
       p hi node
     when 'jade'
       p hi jade
     default
       p hi #{name}
```

## jade中的mixins
十分强大的一个功能，能处理各种数据类型，下面请看代码
```html
doctype html
html
  head
    meta(charset='utf-8')   
    title 转义
  body
    h2 mixin
    mixin lesson
      p imooc jade study
    +lesson
    +lesson
    mixin study(name,courses)
      p #{name} study
      ul.courses
        each course in courses
          li= course
    +study('tom',['jade','node'])
    
    mixin group(student)
      h4 #{student.name}
      +study(student.name,student.courses)
    +group({name:'tom',courses:['jade','node']})
    
    mixin team(slogon)
      h4 #{slogon}
      if block
        block
      else
        p no team
    +team('slogon')
      // 可以去掉看看有啥结果
      p Good job!
      
    mixin attr(name)
      p(class !=attributes.class) #{name}
    +attr('attr')(class='magic')
    mixin attrs(name)
      p&attributes(attributes) #{name}
    +attrs('attrs')(class='magic2',id='attrid')
    
    mixin magic(name,...items)
      ul(class='#{name}')
        each item in items
          li= item
    +magic('magic','node','jade','...')
    
```

## 模板的继承
先看第一种继承方式
```html
doctype html
html
  head
    meta(charset='utf-8')   
    title 继承
  body
    h2 模板继承
    block desc
      p 我是能够被继承的
    block desc
    block desc   
```

再来看看外部继承方式
```html
// layout.jade文件
doctype html
html
  head
    meta(charset='utf-8')   
    title 继承
  body
    block desc
      p desc from layout
    block content
    block desc
    
// 调用的文件
extends layout.jade
block content
  h2 模板继承
```

## jade模板的包含
我们可以将文件的样式、内容、脚本等写成外部文件，然后使用使用include关键字包含进主文件即可，能很有效的提高编程乐趣以及代码复用等
```html
// meta外部文件
meta(charset='utf-8')   
title 包含

// 调用的文件
doctype html
html
  head
    include meta.jade
  body
    h2 包含
    block desc
      p desc from layout
    block desc
```

## jade中render及renderFile方法
这两个是jade的核心API，我们一起来看它们的功能是什么。首先，安装点什么吧：npm install jade coffee-script less markdown --save
```js
var http = require('http')
var jade = require('jade')
http.createServer(function(req, res) {
  res.writeHead(200, {
    // 'Content-Type': 'text/plain'
    'Content-Type': 'text/html'
  })
  // 1、compile()
  // var fn = jade.compile('div #{course}',{})
  // var html = fn({course:'jade'})
  // res.end(html)
  
  // 2、jade.render()
  // var html = jade.render('div #{course}',{course:'jade render'})
  // res.end(html)
  
  // 3、jade.renderFile()
  var html = jade.renderFile('index.jade', { course: 'jade renderFile', pretty: true })
  res.end(html)
}).listen(1337, '127.0.0.1')
console.log('HTTP server runing  http://127.0.0.1:1337')
```

## 过滤器filters
过滤器在jade中就是为了能够使用其他插件的语法，比如使用markdown、less等语法。首先，我们安装点什么：npm install less coffee-script markdown -g
```html
doctype html
html
  head
    meta(charset='utf-8')   
    title 过滤器
  body
    h2 mardown语法
    :markdown
      **imooc** [link](https://www.imooc.com)      
```

## 尾声
好了，今天的总结到此结束，后两个章节的问题我就不在这说了，因为除了版本的问题，现在还有更好的[模板引擎](https://pugjs.org/zh-cn/api/getting-started.html)已经出来了。<br><br>


# 2、VSCode => [VSCode入门](https://www.imooc.com/learn/1106)
看完了课程，发现主要就是以下三点
- 用户配置
- git配置
- 插件配置

之前有写过一篇[类似的分享](https://github.com/CruxF/Blog/issues/12)，就不一五一十的来记录课程的那些了。毕竟以上那三点在网上能找到大量的、更详细的分享，再重复去记录与分享就没太大必要了。

