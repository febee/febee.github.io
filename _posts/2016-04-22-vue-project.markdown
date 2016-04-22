---
layout: post
title:  "Vue的小应用"
author: xiaoyang
categories: [release]
---

# vue应用

主要是将数据传递给父组件，而子组件的更新都是通过父子组件，而子组件之间是没有直接影响的

###1.父组件
父组件主要是通过v-bind:status来绑定数据，.sync主要是来规定数据的双向绑定，可以让各个组件来进行传递，components是用来注册组件

```js
<template>
  <div id="app" v-bind:class.sync="[''+ status + Index]">
     <Tab :status.sync="status"  :Index.sync="Index"></Tab>
     <Book :status.sync="status" :Index.sync="Index"></Book>
  </div>
</template>

<script>
import Tab from './components/Tab.vue'
import Book from './components/Book.vue'

export default {
  data () {
    return {
      status: 1,
      Index: 0
    }
  },
  components: { Tab, Book}
}
</script>

<style>
#app{
	width: 100%;
	height: 100%;
}
</style>
```
###2.子组件
子组件来获取父组件的数据，主要是通过props来传递的，通过$set我们能够改变父组件的数据，从而来渲染其他的组件

```js
<template>
  <div class="left">
       <ul>
        <li>首页</li>
        <li v-for="item of items" :class.sync = "['' + this.status, $index == index ? 'active': '']"  @click = "addClass($index)">{{item}}</li>
       </ul>
  </div>
</template>

<script>
export default {
  props: ['status','Index'],
  data() {
    return {
      items: [ '列表1','列表2','列表3','列表4'],
      index: 0
    }
  },
  methods: {
    addClass(index) {
        if(this.index == index) return false;
        this.index = index;
        this.$set('Index', index);
        this.status = index*100;
    }
  },
  watch: {
    "Index": function(val, oldVal){
         this.index = this.Index;
         this.status = this.index*100;
    }
  }
}
</script>

<style>
/*.left {
  width: 1.80rem;
  height: 100%;
  padding: 0.3rem 0.2rem;
  box-shadow: 0 0 10px #dcdcdc;
  overflow-x: hidden;
  overflow-y: auto;
  
  transition: all .5s ease;
  /*margin-left: -3rem;*/
}
/*.left ul li{
  font-size: 0.4rem;
  height: 0.8rem;
  line-height: 0.8rem;
  width: 1.2rem;
  text-align: center;
  margin: 0 auto;
}*/
.left ul li.active{ 
  background: #dcdcdc;
}
</style>

```