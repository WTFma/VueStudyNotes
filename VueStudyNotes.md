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
$\color{red}{注意}$：插值表达式不能写在html标签中，不能作为值的部分
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

### 4.属性绑定：$\color{#0066ee}{v-bind}$
```
//我们知道插值表达式不能写在html标签内。如果一定要用vue中属性与其绑定，则可以使用v-bind。
<div id="app">
    <a v-bind:href="toBing">ToBing</a>
</div>

<script>
    var vm = new Vue({
        el: '#app',
        data() {
            return {
                toBing: 'http://www.bing.com'
            }
        }, 
        methods: {}
    });
</script>

```
$\color{#ee6600}{知识扩展}$：
- ```组件中的data```如果是一个函数的话，这样每复用一次组件，就会返回一份新的data，类似于给每个组件实例创建一个私有的数据空间，让各个组件实例维护各自的数据。而单纯的写成对象形式，就使得所有组件实例共用了一份data，就会造成一个变了全都会变的结果。
  所以说```vue组件```的data必须是函数。这都是因为js的特性带来的，跟vue本身设计无关。
>- v-bind的简版：$\color{#0069ee}{使用“:”}$
>- ```<div id="app"> <a :href="toBing">ToBing</a></div>  //可以直接省略v-bind用冒号代替。```

### 5.v-once指令
指明此元素的数据只出现一次，数据内容的修改不影响此元素
```
<div id="app">
    <p v-once>{{link}}</p>  //当前这个link只会绑定一次，即从data中首次拿到link的值后，将不再改变。
    <input type="text" v-model="link">  //尽管v-model绑定了link，但由于v-once，这里变了也不影响p标签中的改变了。
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            link: 'http://www.baidu.com'
        },
        methods: {}
    });
</script>
```
### 6.v-html和v-text
- v-html会将vue中属性的值作为html元素来使用
- v-text只会将vuevue中属性的值作为纯文本来使用
```
<div id="app">
    <span v-html="myLink"></span> //这里会把mylink渲染为html格式的标签
    <br>
    <span v-text="myText"></span> //这里把myText直接当做文本内容输出
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            myLink: '<a href="http://www.bing.com">跳转至bing</a>',
            myText: '<a href="http://www.bing.com">渲染的文本</a>'
        },
        methods: {}
    });
</script>
```
## 第三节：事件
事件涉及到参数的赋值、传递、和接收，需要注意。
> ***事件修饰符:*** 在事件处理程序中调用 event.preventDefault() 或 event.stopPropagation() 是非常常见的需求。尽管我们可以在方法中轻松实现这点，但更好的方式是：方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。
为了解决这个问题，Vue.js 为 v-on 提供了事件修饰符。之前提过，修饰符是由点开头的指令后缀来表示的。具体看[官方文档](https://cn.vuejs.org/v2/guide/events.html?#事件修饰符)吧
```
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>
<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
```

### ```click```事件
当发生点击需要进行额外操作时，就触发这个事件
```
<div id="app">
    {{count}}
    <button @click="addCount">Add</button>
    <button @click="redCount">Reduce</button>
    <input type="text" v-model="step"> {{count>10?'大于10':'小于10'}}
</div> 
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            count: 0,
            step: 1
        },
        methods: {
            addCount: function() {
                var mystep = (Number)(this.step); //做一步转换，不然被当做是拼接字符串
                //this.count += this.step;
                this.count += mystep;
            },//根据input的输入增加相应的值
            redCount: function() {
                this.count -= this.step;
            }

        }
    });
</script>
```
### ```mousemove```事件
可以利用这个事件进行鼠标定位等的操作
```
<div id="app">
    <div style="background-color: aquamarine;" v-on:mousemove="GetXY">
        鼠标在颜色区域移动的时候触发,当前x坐标为：{{x}}，y坐标为：{{y}}
        <span style="background-color:coral;width: 100px;" v-on:mousemove="MoveStop">方法一：鼠标至此停止mousemove事件</span>
        <span style="background-color:chartreuse;width: 100px;" @mousemove.stop>方法二：鼠标至此停止mousemove事件</span>
    </div>
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            x: 0,
            y: 0
        },
        methods: {
            //获取当前鼠标所处的坐标
            GetXY(event) {
                this.x = event.clientX;
                this.y = event.clientY;
                console.log(event);
            },
            //停止鼠标移动事件
            MoveStop(event) {
                event.stopPropagation();
            }
        }
    });
</script>
```
### ```keyup```事件及按键修饰符
可以用来做一些信息录入的处理啥的
```
<div id="app">
    <!-- 这样的写法会导致每键入一次，就调用一次事件 -->
    <input type="text" @keyup="EnterOver">
    <!-- 按键修饰符的使用保证输入完毕按下回车才调用事件 -->
    <input type="text" @keyup.enter="EnterOver">
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {},
        methods: {
            EnterOver() {
                alert('输入完毕')
            }
        }
    });
</script>
```
## 第四节：Vue改变内容→虚拟dom和diff算法
### 1.插值表达式的方式
```
<div id="app">
    <!-- 通过插值表达式来获取值，通过事件改变值 -->
    {{count}}
    <button @click="addCount(2)">Add</button>
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            count: 0
        },
        methods: {
            addCount(step) {
                this.count += step;
            }
        }
    });
</script>
```
### 2.计算属性：```computed```
#### Ⅰ.什么是计算属性？
**字面理解**：首先它是属性，即vue这个对象的一个属性，然后拥有计算的能力.
简单来说，计算属性就是一个能$\color{0066ee}{将计算结果缓存起来的}$```属性```（将行为转换为了静态的属性）。
#### Ⅱ.计算属性与方法的区别
```
<div id="app">
    <p>不使用计算属性获取的时间会变化：{{getCurrentTime()}}</p>
    <p>使用计算属性获取的时间不再变化：{{saveCurrentTime}}</p>
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {},
        methods: {
            //获取当前时间
            getCurrentTime() {
                var now = new Date(),
                    y = now.getFullYear(),
                    m = now.getMonth() + 1,
                    d = now.getDate();
                var localTime = y + "-" + (m < 10 ? "0" + m : m) + "-" + (d < 10 ? "0" + d : d) + " " + now.toTimeString().substr(0, 8);
                //每点击一次，时间也随之变化，因为相当于每次点击都要重新执行一遍方法
                console.log('未使用计算属性' + localTime);
            }
        },
        computed: {
            //当我们需要的数据很少进行更改或者很少反复调用，可以选择用computed来将其属性化保存
            saveCurrentTime() {
                var now = new Date();
                //就算多次点击，打印的时间也不会发生改变
                return now
            }
        },
    });
</script>
```
- 执行结果
![计算属性使用情况](/第四节-改变内容与样式/images/4.2计算属性结果.PNG)

#### Ⅲ.结论
调用方法时，每次都需要重新计算，既然有计算过程则必然产生开销，如果结果不经常变化，就可以考虑将结果缓存起来，采用计算属性可以很方便做到这一点；**计算属性的主要特点就是为了将不经常变化的计算结果进行缓存，以节约我们的系统开销**。
$\color{#ee6600}{注意：}$computed里虽然存放的是函数。但在调用时，computed里的东西是一个属性，所以调用时不能使用'()',因为'()'是在调用函数，而不是调用属性。
### 3.watch：监控属性
```watch```用于监控参数变化，并调用函数，newVal是能获得参数新的值，oldVal是参数老的值。
>- 通过watch里给属性绑定函数，当属性值变化，该函数就会自动被调用。调用时可以接收新旧两个参数。

```
<div id="app">
    <p>OverWatch的当前售价为：{{price}}￥。</p>
    你期望的售价是<input type="text" v-model="price">
    <div v-if="isTrue">这是有可能的哦~</div>
    <div v-else>你在做梦吗？</div>
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            price: 198,
            isTrue: true
        },
        methods: {},
        watch: {
            price: function(newPrice, oldPrice) {
                newPrice > 100 ? this.isTrue = true : this.isTrue = false
            }
        },
    });
</script>
```
