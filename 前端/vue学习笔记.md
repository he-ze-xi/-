﻿﻿@[TOC](目录)

> vue学习过程中问题点记录。
> 
![在这里插入图片描述](vue学习笔记.assets/0552acc11df941789bb0973d0c970037.jpeg)
## 一.created()和mounted()的区别及其用法
> 在Vue.js中，created()和mounted()是两个常见的生命周期钩子函数。具体用法和区别如下：<br>
> **created()方法**：在Vue实例被创建之后立即执行。在这个阶段，Vue实例的数据观测和事件配置已完成，但尚未挂载到DOM上。通常在这个阶段执行一些数据初始化、事件监听、异步请求等逻辑，但不涉及DOM操作。<br>
> **mounted()方法**：在Vue实例挂载到DOM之后执行。在这个阶段，Vue实例已经完成了数据观测、编译渲染、创建虚拟DOM和真实DOM等所有过程，可以进行DOM操作。通常在这个阶段执行一些需要依赖DOM元素的逻辑，如获取元素尺寸、绑定事件、设置定时器等。

```javascript
<script>
export default {
  data() {
    return {
      message: 'Hello, World!'
    }
  },
  created() {
    console.log('created:', this.message); // 'created: Hello, World!'
    // 执行一些数据初始化、事件监听等逻辑
  },
  mounted() {
    console.log('mounted:', this.$el.textContent); // 'mounted: Hello, World!'
    // 执行一些需要依赖DOM元素的逻辑，如获取元素尺寸、绑定事件、设置定时器等
  }
}
</script>
```

> 总结：需要注意的是，created()和mounted()只有在Vue实例被成功创建和挂载后才会执行，如果中间发生了错误或中止，那么这两个函数也不会执行。在使用这两个函数时，要避免过多的依赖全局状态或其他模块，以保证逻辑的灵活性和可重用性。
## 二.vue生命周期函数
### 1.什么是生命周期
> vue的生命周期是指：从vue实例产生开始到vue实例被销魂这段时间所经历的过程。

> vue实例的创建和销毁过程好比人的一生从出现到死亡过程。在其中一些重大经历，也就是特殊时间点，我们可以做什么事情。<br>
在vue的一生中，从vue组件创建开始一直到其被销毁时的时间段中，也被建立了几个特殊的时间点，为了方便开发者实现在特定的时候让vue在特定的时间做特定的事情。通过一个叫做“生命周期钩子函数”的方法进行设置。在vue中，vue的一生被8个生命周期钩子函数给支配着。<br>
也可以理解为回调函数，当我们写了周期函数后，在规定的时间点，vue会自动执行这个函数。<br>
钩子函数：可以简单理解为是一种系统在不同的阶段自动执行、用户无须干预的函数。<br>
### 2.生命周期简介
> vue的生命周期事件主要被分为2个钩子：事件之前和事件之后调用。具体来说，可分为4种类型的事件：<br>
>  ● 创建–在组件创建时执行 。
> ● 挂载–DOM被挂载时执行 。
> ● 更新–当响应数据被修改时执行 。
> ● 销毁–在元素被销毁之前立即执行。
> 
|  函数| 简介 |
|--|--|
|beforeCreate|在 VUE 实例生成之前会自动执行的函数。|
|created|在 VUE 实例生成之后会自动执行的函数。|
|beforeMount|在组件内容被渲染到页面之前自动执行的函数。|
|mounted|在组件内容被渲染到页面之后自动执行的函数。|
|beforeUpdate|当data中的数据发生变化前执行的函数。|
|updated|当data中的数据发生变化后执行的函数。|
|beforeUnmount|VUE实例与元素解除绑定前执行的函数。|
|unmounted|VUE实例与元素解除绑定后执行的函数。|

> 代码举例：

```javascript
<script src="../vue.global.js"></script>
</head>
<body>
    <div id="myDiv"></div>
</body>
<script>
 
    // 生命周期函数：在某一时刻会自动执行的函数
    const app = Vue.createApp({     // 创建一个vue应用实例
        data() {
            return {
                message : "hello"
            }
        },
        //在实例生成之前会自动执行的函数
        beforeCreate() {
            alert("beforeCreate")
        },
        //在实例生成之后会自动执行的函数
        created() {
            alert("created")
        },
        // 在组件内容被渲染到页面之前自动执行的函数
        beforeMount() {
            alert("beforeMount：" + document.getElementById("myDiv").innerHTML)
        },
        // 在组件内容被渲染到页面之后自动执行的函数
        mounted() { // 绑定后自动执行
            alert("mounted：" + document.getElementById("myDiv").innerHTML)
        },
        // 当data中的数据发生变化前执行
        beforeUpdate() {
            
            alert("beforeUpdate：" + document.getElementById("myDiv").innerHTML);
        },
        // 当data中的数据发生变化后执行
        updated() {
            alert("updated：" + document.getElementById("myDiv").innerHTML);
        },
        // 解除绑定前执行的函数
        beforeUnmount() {
            alert("beforeUnmount：" + document.getElementById("myDiv").innerHTML);
        },
        // 解除绑定后执行的函数
        unmounted() {
            alert("unmounted：" + document.getElementById("myDiv").innerHTML);
        },
        // 如果没有 template ，则取绑定元素下面的子元素作为 template
        template : "<div>{{message}}</div>"
    });
 
    // vm 就是vue应用的根组件
    const vm = app.mount('#myDiv');  // 绑定id为myDiv的元素
 
    // 更新数据
    vm.$data.message = 'hello world!!!';
 
    // 解除绑定
    app.unmount();
</script>
```
## 三.
