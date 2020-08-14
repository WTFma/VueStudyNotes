# Vue学习笔记
***
## 第一节 ：Why Vue?

Vue是一个$\color{#0066ee}{渐进式}$的js框架。更多的注重视图层，结合了HTML+CSS+JS，易用，有良好的生态。最主要是它体积小呀，速度快（虚拟Dom），优化搞得也可以。
> - **渐进式**：个人理解为搭积木的感觉，用到多少拿多少。根据需求，利用生态。借助已有的工具和库搭建自己的项目，成本小、效率高。

### Vue所采用的开发模式
**Vue技术是MVVM开发模式的$\color{#0066ee}{实现者}$。**
|                   M                   |                        V                         |                         VM                         |
| :-----------------------------------: | :----------------------------------------------: | :------------------------------------------------: |
| Model，模型层（简单的理解为数据部分） | View，视图层（简单的理解为页面所渲染展示的部分） | ViewModel 视图模型（将模型和视图进行关联的中间件） |
在MVVM架构中，是不允许数据和视图直接通信的，只能通过```ViewModel```来通信，而ViewModel就是定义了一个```observer```观察者。
>Vue.js有很好的数据双向绑定功能，类似的有Angular，不过其实现原理截然不同，Vue.js利用的是数据劫持的方法，而Angular使用的是类似脏值检查的实现。这里的数据劫持的含义是Vue.js根据发布者/订阅者模式的原理，利用文档碎片化对各属性所具有的setter和getter进行劫持，当监听的数据产生变化，订阅者会就收到变化的消息，诱发监听回调。

由上可知，Vue.js核心就是实现了```DOM监听```和```Data Binding```。

### CDN加速策略
**CDN(Content Delivery Network)**:内容分发网络。我们需要的网络资源，如Vue.js的源文件可以利用script从网络上获取，当资源的加载不便（源文件在外国服务器）时就可以通过分布在世界各地的有这个资源的其它服务器进行获取。简单的理解为分布式存储这个源文件，优先获取与你距离相近的服务器上的资源。

### 组件化
- 页面上的每个功能模块可以视为一个组件
- 每个组件对应一个工程目录，组件所需的资源在这个目录下就近维护
- 页面不过是组件的容器，组件可以自由组合（复用）形成完整的页面

### 为什么使用Vue
- 轻量；
- 移动优先，更适合移动端，比如移动端的Touch事件；
- 易上手，学习曲线平稳，文档齐全；吸取了Angular（```模块化```）和React（```虚拟DOM```）的长处，并拥有自己独特的功能，如```计算属性```；
- 开源，社区活跃
***
## 第二节：Let`s Begging
- 1.在项目中引入Vue的js文件
  方法一：使用CDN策略进行引用

  ``` <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> ```

  方法二：下载/保存Vue的js文件，然后进行本地引用

  ``` <script src="项目目录/.../vue.js"></script> ```

- 2.页面上进行Vue元素的绑定
  ```
  在body中创建一个div，id为app
  <div id="app"></div>
  ```
- 3.创建Vue对象（实例），进行对象内容的包装
  ```
  <script>

    var myApp = new Vue({
        el:'#app',//element
        //数据从何而来？
        data:{
            name:'Ma',
            age:22
        }//数据是通过发送ajax请求，请求后端提供api接口，拿到相应的数据。
    });//json格式的对象 使用大括号包裹，里面放入了键值对，在js中键可以没有引号
  
  </script>
  ```

- 4.在页面元素中通过插值表达式来获取Vue对象中的内容

  ```

  <div id="app">
        hello,{{name}},your age is {{age}}
  </div>
  ```

### 1.插值表达式
插值表达式使用在html中被绑定的元素中的。目的是通过插值表达式从Vue实例中获取vue对象的属性（```data```）和方法（```method```）
```
new Vue({
    data:{}  <==这个data提供了属性
    methods:{   <==这个methods提供了方法
        SayHi(){}
    }
})
```
>- 此外，插值表达式也能够这样使用：
>```
><div id="app">
>   {{[0,1,2,3][1]}} //show 1
>   {{ {major:'C#',credit:2}.major }} //show C#  
></div>
>```

### 2.MVVM双向数据绑定：$\color{#0066ee}{v-model}$
```
<div id="app">
    <input type="text" v-model="name" /> //这里v-model会与vue实例中的name这个属性绑定，当两者任意方改变，另外一方也会随之改变
</div>
```

### 3.事件绑定：$\color{#0066ee}{v-on}$
```
<div id="app">
    <input type="text" v-on:input="changeName("Wang")" /> //文本框中输入时，就会调用对应事件changeName(parameter)
</div>
```
```
<script>

    var myApp = new Vue({
        el:'#app',
        data:{
            name:'Ma',
            age:22
        },
        methods:{
            SayHi(){},
            changeName(new_name:string){
                this.name = new_name //通过参数传递过来的新的名字
                console.log('name is changed！')
            }, //改变Vue的name这个属性的值
            getEventValue:function(event){
                console.log(event.target.value) //这个方法是无参的，event为内置事件。打印event事件对象的value值
            }

        }
    });
  
</script>

``` 

>- v-on的简版：$\color{#0069ee}{使用@}$
>
>```
><div id="app">
>    <input type="text" @input="getEventValue" /> //文本框中输入时，就会调用getEventValue（无参）这个事件对象，获取其value值
></div>
>```
$\color{#ee6600}{补充知识}$：
1. 在响应函数里，可以指明event内置的参数对象。该对象表示当前的事件，可以通过```event.target.value```来获得当前事件对象的value的值。
2. Vue实例中的this，表示当前Vue对象可以通过“this.”来调用当前vue对象的属性和方法。