---
layout: post
title:  "lodash学习之数组篇（一）"
author: 邓加鹏
categories: [release]
---


lodash为我们提供了很多工具函数，它能帮助我们更方便的处理 arrays,numbers,objeacts,strings
等，它具有如下优点：
    *方便的迭代数组，对象与字符串。
    *创建复用的函数。
    *方便操控与测试。


下面及以后的文章我会记录我的学习过程：

## 安装
    
基于commonJs规范的安装：
    npm install lodash --save

```js
    //如果你不关心文件大小，你可以把所有模块加载进来:
    import _ from 'lodash'

    //基于commonjs的规范，当然你一可以单独的引入摸一个模块
    import array from 'lodash/array'
```
单独对某些模块的引入可以减少你应用文件的大小，这一点很重要，
这也是为什么很多框架都把自己拆成很多包分别发布到npm上，如<br />
react的react-dom,react-addons-css-transition-group....，当然
除了拆npm包之外，你可以单独去引模块你的某个文件，这是基于commonjs
的模块应用规则，例如：<br>
    require('lodash/array') 

## API

下面就来具体学习lodash吧
    
### arrays

    _.chunck(array,[size=0]);

    ```js
        _.chunk(['a','b','c','d'],4);//=>[['a','b','c','d']]
        _.chunk(['a','b','c','d'],0);//=>[]
        _.chunk(['a','b','c','d'],2);//=>[['a','b'],['c','d']];
    ```

    
    _.compact(array);创建一个新数组，把所有为假的元素去除，识别为
    假的元素有 false,null,0,"",undefined,NaN

    ```js
        _.compact([0,1,false,NaN,'',undefined,null])//=>[1]
    ```


    _.concat(array,[values]);把一个数组和其他数组或其他额外的值链接起来。

    ```js
        var array1 = [1];
        var other1 = _.concat(array1,2,[3],[[4]],{a:1});
        console.log(array1);//[1],说明这个方法不会污染原来的数组
        console.log(other1);//[1,2,3,[4],{a:1}]

        /**
         * 对比array对象的concat方法
         */
        var array2 = [1];
        var other2 = array2.concat(2,[3],[[4]],{a:1});
        console.log(array2);//[1],同样不会污染原来的数组
        console.log(other2);//[1,2,3,[4],{a:1}]和上面的几乎一样。
    ```


    _.difference(array,[values]);//用第一个数组去和后面的值比较，比较完之后不同的留下，有相同的去掉，

    ```js
        console.log(_.difference([3,2,1],[4,2],[22,44],[1]));//=》[1]
    ```


    _.differenceBy(array,[values],[iteratee=_.identity]);和上面的一样，只是为
    每个元素提供了一个钩子（函数，对象，或字符串~~对象，字符串做钩子还是第一次
    见，）
    
    ```js
        console.log(_.differenceBy([3.1,2.9,8.1],[3.0],Math.floor));//=>[2.9,8.1]

        console.log(_.differenceBy([{ 'x': 2,'y': 2 }, { 'x': 1 ,'y':3}], [{ 'x': 1,'y': 3 }],'y'));//=>[{'x':2,'y':2}],'y'表示以对象的那个键值进行比较
    ```


    _.differenceWith(array,[values],[comparator]);这个方法和difference很像，只是这个方法提供了
    钩子函数比较两个参数

```js
    var objects = [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }];

    console.log(_.differenceWith(objects, [{ 'x': 1, 'y': 2 }], _.isEqual));
    //=>[{'x':2,'y':1}],这边是一个对象比较的问题，如果按常规比较，{}!={};但是实际情况下，我们需要
    {}=={}，及他的键值相等。_.isEqual就是这个作用。
```


    _.drop(array,[n=1]);相当于array的slice，从第n个元素开始（之后）切割数组（不会影响到原来的数组，返回一个新数组）

```js
    console.log(_.drop([1,2,3,4],1));//[2,3,4]
    console.log(_.drop([1,2,3,4],0));//[1,2,3,4]
    console.log(_.drop([1,2,3,4],5));//[]
```

    _.dropRight(array, [n=1]);和上面的一样，不过刚才是从左边开始，现在是从右边开始

```js
    console.log(_.dropRight([1,2,3,4],1));//[1,2,3]
    console.log(_.dropRight([1,2,3,4],0));//[1,2,3,4]
    console.log(_.dropRight([1,2,3,4],5));//[]
```


    _.dropRightWhile(array, [predicate=_.identity])还是和上面一样，切割数组知道predicate返回false

```js
    
    var users = [
        {'user':'barney1','active':false},
        {'user':'barney2','active':true},
        {'user':'fred','active':false},
        {'user':'pebbles','active':false}
    ];

    console.log(_.dropRightWhile(users,(item) => !item.active))
    //到了barney这个对象才返回false，取到的是banery1，barney2，所以自己体会他的取值规则吧
    
    console.log(_.dropRightWhile(users,{'user':'pebbles',active:true}));//[barney1,barney2,fred,pebbles]对应的对象

    console.log(_.dropRightWhile(users,{'user':'pebbles',active:false}));//[barney1,barney2,fred]对应的对象
    //这种是才有_.matches()去匹配的，匹配为假就停下，并且是匹配，不是相等。

    console.log(_.dropRightWhile(users,['active',false]));//[barney1,barney2]对应的对象
    //_.matchesProperty去匹配，匹配为false就停下

    console.log(_.dropRightWhile(users, 'active'));
    //_.property去匹配,


```


    _.dropWhile(array, [predicate=_.identity]);和上面几乎一样，只是这个从左边开始。

    

    _.fill(array, value, [start=0], [end=array.length]);用value去填充array的元素，从start开始到end结束（不包括开始哪一位）

```js
    var array = [1,2,3];

    console.log(_.fill(array,'a'));//['a','a','a']

    console.log(_.fill(Array(3),'b'));//['b','b','b']
```


## 小结

知识每天学一点，下期继续
