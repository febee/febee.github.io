---
layout: post
title:  "js原生Dom操作复习"
author: 邓加鹏
categories: [release]
---

# 原生DOM操作复习

小伙伴们还记得原生js怎么操作DOM吗？如果你回答忘了，那么是时候捡起你的老本行了。

## 为什么？也许你会问,jquery不是用的挺好的吗？

No！No！No！随着react在项目中慢慢普及，你将会慢慢的体会到，你对jquery的依赖程度会越来越低。

jquery给我们带来便利是命令式的Dom操作，以及一些全局方法（如ajax，extend等，），当然
还有一大堆别人的轮子。

但是，好的react Coding将会让你免去dom的操作，而且CommonJS的规范可以放你随心所欲的import其他
独立小巧的库（如fetch替代ajax），而不是一大坨jquery源码。    

## 哪然后哩？

进入正题吧，如果你的react Coding 写的不是那么优雅，或者某种原因让你不得不在mounted的函数
中操作DOM,如果你require('jquery');那没问题，但是看看jquery的文件大小吧，为了几个dom操作就要引入
几百k的文件有点得不偿失，所以是时候回顾一下JS原生都没的操作了:


###node 的属性操作:

任何操作之前你要获取结点：
```js
getElementById() //单独的DOM对象
getElementsByTagName() //DOM对象的集合
getElementsByName() //DOM对象的集合
getElementsByClassName() //DOM对象的集合
```

有了DOM对象，我就可以操作我自身属性了：
```js
innerHTML
innerText //这两个只有body一下才有的
parentNode
firstChild
lastChild
nextSibling
previousSibling
childNodes
attributes
style 
tagName
```

对该节点的操作
```js
replaceChild() //结点交还
setAttribute() 
getAttribute() 
removeChild(element)
removeAttribute()
```
新增结点：
```js
document.createElement()
createTextNode()
appendChild(element)
insertBefore(newelement,childelement) //在指定子节点childelement之前插入childelement。
```

## 小结

因为react组件的模块化，所以涉及DOM操作也不会复杂，感觉能用到的就这些。对了你想要获取组件的根节点怎么办<br />：
只需要：ReactDom.findDOMNode(this)即可：
```js
componentDidMount() {
    console.log(ReactDom.findDOMNode(this));
}
```





