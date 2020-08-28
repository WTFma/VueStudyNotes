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
插值表达式获取Vue对象的中的属性值来自于data、computed、props

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

## 第五节：vue改变样式
### 1.class的动态绑定
通过给html元素的class属性绑定vue中的属性值，得到样式的动态绑定
```
<style>
    .mydiv {
        width: 400px;
        height: 220px;
        background-color: #ee66ee;
    }
    
    .red {
        background-color: red;
    }
    
    .green {
        background-color: green;
    }
</style>
<div id="app">
    <!-- 如果isShow为真 则class="red"否则class不做绑定 -->
    <div class="mydiv" :class="{red:isShow}"></div>
    <button @click="changeColor">点击变红</button>
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            isShow: false
        },
        methods: {
            changeColor() {
                this.isShow = !this.isShow
            }
        }
    });
</script>
```

### 2.通过computed绑定
通过computed返回一个对象，对象里面存放着多个键值对。
```
<style>
    .mydiv {
        width: 400px;
        height: 220px;
        background-color: #ee66ee;
    }       
    .red {
        background-color: red;
   }        
    .myWidth {
        width: 500px;
    }
</style>
<div id="app">
    <!-- 如果isShow为真 则class="red"否则class不做绑定 -->
    <div class="mydiv" :class="{red:isShow}"></div>
    <button @click="changeColor">点击变红</button>
    <hr>
    <!-- 通过计算属性绑定所需的对象给class -->
    <div class="mydiv" :class="changeWC"></div>
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            isShow: false
        },
        methods: {
            changeColor() {
                this.isShow = !this.isShow
            }
        },
        computed: {
            changeWC() {
                return {
                    red: this.isShow,
                    myWidth: this.isShow
                }
            }
        },
    });
</script>
```

### 3.多个样式绑定及style绑定
设置div的style属性的值，style里放json对象，键是驼峰命名法，值是对应的样式。比如:“:style={backgroundColor:'对应的data'}”
```
<style>
    .mydiv {
        width: 400px;
        height: 220px;
        background-color: #ee66ee;
    }
    
    .red {
        background-color: red;
    }
    
    .myWidth {
        width: 500px;
    }
</style>
<div id="app">
    <div :class="[useWidth,useRed]" class="mydiv">多样式绑定</div>
    <!-- 注意：针对使用v-bind绑定的样式不能这样写 -->
    <!-- <div :class="useWidth" :class="useRed"></div> -->

    <hr>
    <!-- 通过style设置样式 -->
    <!-- 注意：style引用了vue中的内容，因此是一个键值对，所以需要大括号，json格式。json格式内部不允许使用“-”，所以对象的键要注意 -->
    <div :style="{backgroundColor:bc}" class="mydiv">通过style设置样式</div>
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            useWidth: 'myWidth',
            useRed: 'red',
            bc: 'blue'

        },
        methods: {}
    });
</script>
```

### 4.结合computed在style里使用多个样式的数组
一个绑定改变多个样式需要使用数组“[ ]”进行
```
<style>
    .mydiv {
        height: 200px;
        background-color: aqua;
    }
</style>
<div id="app">
    <!-- comWidth使用的是computed绑定，另外使用的是一般的style对象绑定 -->
    <div :style="[comWidth,{backgroundColor:newColor}]" class="mydiv"></div>
    <hr> 改变宽度<input type="text" v-model="daWidth">(自动添加px单位)
    <hr> 改变颜色
    <input type="text" v-model="newColor">
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            daWidth: 120,
            newColor: '#0066ee'
        },
        methods: {
            changeWidth() {
                console.log(this.daWidth)
            }
        },
        computed: {
            comWidth: function() {
                return {
                    width: this.daWidth + 'px'
                }
            }
        },
    });
</script>
```

## 第六节：Vue的核心，虚拟DOM和diff算法
Vue高效的核心，就是虚拟的dom和diff算法，vue不通过修改dom来达到修改的效果，而是直接在页面上改那个元素，此时这个元素就是一个虚拟的dom。
那vue如何修改呢？通过diff算法，计算出虚拟的dom修改前和修改后的区别，然后在虚拟的dom的原基础上修改，这样效率就很高了。

## 第七节：Vue分支语句
v-if:
- 语法：v-if="vue-data（true）"。直接渲染新htnl元素

v-else
- v-if的对立面

v-else-if
-  是在else里面再嵌套一个if

v-show
- 控制html元素是否显示。效率高，因为只是在元素本身加了一个dispaly=none的属性。

$\color{red}{注意：}$v-show不能作用于template及其子元素；而v-if可以。简单来说就是：v-if写在template上时，其本身和子元素都不会被渲染。v-show会渲染template其子元素。
```
<div id="app">
    <p>你期待今年的NBA总决赛吗？？</p>
    <button @click="DreamTrue"> 当然</button>
    <button @click="DreamFalse"> 并不</button>
    <div v-if="isDiscont">恭喜获得前往现场观看NBA的门票！</div>
    <div v-else>抱歉，或许你对其他体育赛事感兴趣。</div>
    <hr>
    <div v-show="isInterest">对NBA感兴趣~</div>
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            isDiscont: false,
            isInterest: true
        },
        methods: {
            DreamTrue() {
                this.isDiscont = true;
            },
            DreamFalse() {
                this.isDiscont = false;
            }
        }
    });
</script>
```
## 第八节：v-for
### 一般用法
```
<style>
    .myColor {
        color: crimson;
    }
</style>
<div id="app">
    红色为偶数
    <ul>
        <li v-for="num in nums"><span :class="[num%2==0?'myColor':null]">{{num}}</span></li>
    </ul>
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            nums: [55, 66, 77, 88, 99],
        },
        methods: {},
    });
</script>
```
### 和模板结合使用
v-for在table中和template的组合使用：4定
> 1. 定义组件及其逻辑
> 2. 定位要渲染的html
> 3. 定template的方式书写使用
> 4. 定点使用组件
```
<style>
    .myColor {
        color: crimson;
    }
</style>
<div id="app">
    <table>
        <thead>
            <tr>
                <th>姓名</th>
                <th>年龄</th>
                <th>电话</th>
                <th>操作</th>
            </tr>
        </thead>
        <tbody>
            <!-- ④使用组件 -->
            <tr is="tabcontent" v-for="user in users" :u="user"></tr>
        </tbody>
    </table>

</div>
<!-- ③以template的方式书写使用 -->
<template id="temp-table-content">
    <tr>
        <td>{{u.name}}</td>
        <td :class="[u.age>20?'myColor':null]">{{u.age}}</td> 
        <td>{{u.tel}}</td>
        <td>
            <button @click="SayHi(u.name)">说话</button>
        </td>          
    </tr>
</template>
<script>
    //①定义组件及其逻辑
    Vue.component('tabcontent', {
        props: ['u'],
        methods: {
            SayHi(name) {
                alert("你好" + name)
            }
        },
        //②定位要渲染的html
        template: '#temp-table-content'
    });
    var vm = new Vue({
        el: '#app',
        data: {
            //虚拟数据
            users: [{
                name: 'Tim',
                age: 29,
                tel: '1888888888'
            }, {
                name: 'Nano',
                age: 26,
                tel: '1777777777'
            }, {
                name: 'Aluoha',
                age: 18,
                tel: '1666666666'
            }]
        },
    });
</script>
```

## 第九节：组件
**VUE很重要的一大特性就是组件化。** vue组件可以将vue对象作为一个组件，通过组件可以实现模块的复用，提高程序的灵活性，利于组织开发。
### 1.组件注册
#### 1-1.全局注册
在被vue绑定了的html元素中才能使用组件。如果一个div没有被vue绑定，那么这个div不能使用之前注册的组件。
```
<div id="app">
    <component-a></component-a>
    <!-- 组件复用因为data是个函数有很好的隔离性 -->
    <component-a></component-a>
</div>
<script>
    Vue.component('component-a', {
        data() {
            return {
                title: '组件A',
                num: 0
            }
        },
        methods: {
            CountAdd() {
                this.num += 1
            }
        },
        template: `<div>
            <p>hello{{title}}</p>
            <button @click="CountAdd">num is {{num}}</button>
        <div>` //template中直接写的话需要使用“ `` ”对html元素进行包裹。
    })
    var vm = new Vue({
        el: '#app',
        data: {},
        methods: {}
    });
</script>
```

#### 1-2.局部注册

Vue中的组件可以扩展HTML元素，用于封装可复用的代码，但是全局组件不需要挂载，但是不是很常用，尽量少在全局上使用组件，这样的话会影响浏览器的性能，而局部组件必须要手动挂载，不然会没有效果
什么场景使用？如果不需要全局注册，或者是让组件使用在其它组件内，可以用选项对象的 components 属性实现局部注册。
1. 以JavaScript 对象来定义组件
 ```
 var ComponentA = { /* ... */ }
 var ComponentB = { /* ... */ }
 ```
2. 在 components 选项中定义你想要使用的组件
```
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```

### 2.组件传值
#### prop
prop类似一个传输介质，连接父子组件的通信，当然这个通信过程是单向的，就像瀑布，父组件的值会$\color{red}{单向传递}$给子组件，以供其使用。
$\color{#ee6600}{知识扩展}$：单向传递是为了防止从子组件意外变更父级组件的状态，从而导致你的应用的数据流向难以理解。每次父级组件发生变更时，子组件中所有的 prop 都将会刷新为最新的值。所以不应该在一个子组件内部改变 prop。
> prop的类型？归定props类型是有好处的，较强的类型毕竟还是好。
> ```
> props: {
>  title: String,
>  likes: Number,
>  isPublished: Boolean,
>  commentIds: Array,
>  author: Object,
>  callback: Function,
>  contactsPromise: Promise // 或是任何其他的构造函数
> }
> ```
**利用props进行组件的值传递。**
```
<div id="app">
    <!-- 遍历人员信息且将人员信息传递给子组件 -->
    <component-a v-for="(person,index) in persons" :person="person" :index="index"></component-a>
</div>
<script>
    Vue.component('component-a', {
        props: { //使用props类型限制可以很好杜绝一些非法的值
            person: {
                type: Object
            },
            index: {
                type: Number
            }
        },
        data() {
            return {
                //对接收到的值进行一些简单处理
                comPerson: this.person,
                comIndex: this.index
            }
        },
        template: `
        <div>
            <ul>The Person info of number {{comIndex+1}} is 
                <li>Name===>{{comPerson.name}}</li>
                <li>Age===>{{comPerson.age}}</li>
            </ul>
        <div>
        ` //template中直接写的话需要使用“ `` ”对html元素进行包裹。
    })

    var vm = new Vue({
        el: '#app',
        data: {
            persons: [{
                name: 'Ma',
                age: 23,
                tel: '15088888999'
            }, {
                name: 'Zhang',
                age: 23,
                tel: '18888888888'
            }]
        },
        methods: {}
    });
</script>
```

### 3.子传父
> 四流程
> 1. 子组件定义发送事件
> 2. 使用$emit进行发送
> 3. 父组件使用“@+emit定义的回调函数名称”监听回调函数，并且执行对应操作
> 4. 父组件获取子组件值并且操作
```
<div id="app">
    <p>父组件的值：{{fatherInfo}}</p>
    <!-- ③父组件监听回调且执行对应的事件，该事件自带参数，参数值就是子组件传上来的值 -->

    <!-- 使用函数处理回调 -->
    <gou-wu-che @aluha="GetInfo"></gou-wu-che>
    <!-- 直接将子值赋值给父值 -->
    <gou-wu-che @aluha="fatherInfo=$event"></gou-wu-che>

</div>

<template id="GWC">
    <div>
        {{CCInfo}}
        <!-- ①在子组件中定义发送事件 -->
        <button @click="SendMoney(CCInfo)">click</button>
    </div>
</template>
<script>
    Vue.component('gou-wu-che', {
        data() {
            return {
                CCInfo: 99
            }
        },
        methods: {
            //②使用this.emit()准备将数据向上传递
            //第一个参数是父组件监听的回调函数名称（最好要小写，大写容易抽风）,第二个参数是要传递的值
            SendMoney(child_info) {
                this.$emit("aluha", child_info)
            }
        },
        template: '#GWC',

    })
    var vm = new Vue({
        el: '#app',
        data: {
            fatherInfo: 0
        },
        methods: {
            //④父组件执行
            GetInfo: function(data) {
                alert("xx的价格为：" + data + "＄")
            }
        }
    });
</script>
```

## 第十节：Vue的生命周期
### 1.什么是vue生命周期？
> Vue 实例从创建到销毁的过程，就是生命周期。也就是从开始创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、卸载等一系列过程.
 
### 2.vue生命周期的作用是什么？
>vue的生命周期中有多个事件钩子，让我们在控制整个Vue实例的过程时更容易形成好的逻辑
### 3.每个周期具体适合哪些场景？
> - beforecreate : 可以在这加个loading事件，在加载实例时触发 
> - created : 初始化完成时的事件写在这里，如在这结束loading事件，异步请求也适宜在这里调用.做一些数据初始化，实现函数自执行
> - mounted : 挂载元素，获取到DOM节点,调用后台接口进行网络请求，拿回数据，配合路由钩子做一些事情。
> - updated : 如果对数据统一处理，在这里写上相应函数 
> - beforeDestroy : 可以做一个确认停止事件的确认框 
> - nextTick : 更新数据后立即操作dom

### 4.详解每个周期
*原文出处：https://www.jianshu.com/p/672e967e201c*
#### 4.1 beforeCreate( 创建前 )
在实例初始化之后，数据观测和事件配置之前被调用，此时组件的选项对象还未创建，el 和 data 并未初始化，因此无法访问methods， data， computed等上的方法和数据
#### 4.2 created ( 创建后 ）
实例已经创建完成之后被调用，在这一步，实例已完成以下配置：数据观测、属性和方法的运算，watch/event事件回调，完成了data 数据的初始化，el没有.然而，挂在阶段还没有开始, \$el属性目前不可见，这是一个常用的生命周期，因为你可以调用methods中的方法，改变data中的数据，并且修改可以通过vue的响应式绑定体现在页面上，，获取computed中的计算属性等等，通常我们可以在这里对实例进行预处理，也有一些童鞋喜欢在这里发ajax请求，值得注意的是，这个周期中是没有什么方法来对实例化过程进行拦截的，因此假如有某些数据必须获取才允许进入页面的话，并不适合在这个方法发请求，建议在组件路由钩子beforeRouteEnter中完成.
#### 4.3 beforeMount
挂在开始之前被调用，相关的render函数首次被调用（虚拟DOM），实例已完成以下的配置： 编译模板，把data里面的数据和模板生成html，完成了el和data 初始化，注意此时还没有挂在html到页面上。

#### 4.4 mounted
挂载完成，也就是模板中的HTML渲染到HTML页面中，此时一般可以做一些ajax操作，mounted只会执行一次。

#### 4.5 beforeUpdate
在数据更新之前被调用，发生在虚拟DOM重新渲染和打补丁之前，可以在该钩子中进一步地更改状态，不会触发附加地重渲染过程
#### 4.6 updated（更新后）
在由于数据更改导致地虚拟DOM重新渲染和打补丁只会调用，调用时，组件DOM已经更新，所以可以执行依赖于DOM的操作，然后在大多是情况下，应该避免在此期间更改状态，因为这可能会导致更新无限循环，该钩子在服务器端渲染期间不被调用

#### 4.7 beforeDestroy（销毁前）
在实例销毁之前调用，实例仍然完全可用
>1. 这一步还可以用this来获取实例，
> 2. 一般在这一步做一些重置的操作，比如清除掉组件中的定时器 和 监听的dom事件

#### 4.8 destroyed（销毁后）
在实例销毁之后调用，调用后，所以的事件监听器会被移出，所有的子实例也会被销毁，该钩子在服务器端渲染期间不被调用

### 5.code
```
<div id="app">
    <p>{{ message }}</p>
    <button @click="Change">点击观察</button>
    <p v-if="embodimentOfDiff">再次点击你会发现控制台不会改变，也一定程度体现了vue的diff算法</p>
</div>
<script src="https://cdn.bootcdn.net/ajax/libs/vue/2.6.11/vue.min.js"></script>
<script type="text/javascript">
    var app = new Vue({
        el: '#app',
        data: {
            message: '这个vue生命周期钩子函数讲解的很棒,通俗易懂,值得关注,收藏 '
        },
        methods: {
            Change() {
                this.message = "原有的message已经被改变了",
                this.embodimentOfDiff = true
            }
        },
        beforeCreate: function() {
            console.group('beforeCreate 创建前状态===============》');
            console.log("%c%s", "color:red", "el     : " + this.$el); //undefined
            console.log("%c%s", "color:red", "data   : " + this.$data); //undefined 
            console.log("%c%s", "color:red", "message: " + this.message)
        },
        created: function() {
            console.group('created 创建完毕状态===============》');
            console.log("%c%s", "color:red", "el     : " + this.$el); //undefined
            console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化 
            console.log("%c%s", "color:red", "message: " + this.message); //已被初始化
        },
        beforeMount: function() {
            console.group('beforeMount 挂载前状态===============》');
            console.log("%c%s", "color:red", "el     : " + (this.$el)); //已被初始化
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化  
            console.log("%c%s", "color:red", "message: " + this.message); //已被初始化  
        },
        mounted: function() {
            console.group('mounted 挂载结束状态===============》');
            console.log("%c%s", "color:red", "el     : " + this.$el); //已被初始化
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data); //已被初始化
            console.log("%c%s", "color:red", "message: " + this.message); //已被初始化 
        },
        beforeUpdate: function() {
            console.group('beforeUpdate 更新前状态===============》');
            console.log("%c%s", "color:red", "el     : " + this.$el);
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data);
            console.log("%c%s", "color:red", "message: " + this.message);
        },
        updated: function() {
            console.group('updated 更新完成状态===============》');
            console.log("%c%s", "color:red", "el     : " + this.$el);
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data);
            console.log("%c%s", "color:red", "message: " + this.message);
        },
        beforeDestroy: function() {
            console.group('beforeDestroy 销毁前状态===============》');
            console.log("%c%s", "color:red", "el     : " + this.$el);
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data);
            console.log("%c%s", "color:red", "message: " + this.message);
        },
        destroyed: function() {
            console.group('destroyed 销毁完成状态===============》');
            console.log("%c%s", "color:red", "el     : " + this.$el);
            console.log(this.$el);
            console.log("%c%s", "color:red", "data   : " + this.$data);
            console.log("%c%s", "color:red", "message: " + this.message)
        }
    })
</script>
```

## 第十一节：使用Axios
> 1. 通过cdn引入Axios的包：\<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
> 或者在cli下使用npm：$ npm install axios
> 2. 在Vue实例挂载的时候进行请求

```
<div id="app">
    <p>控制台查看本机位置信息</p>
    <button @click="GetLocation">获取</button>
</div>
<script>
    // 开发者 key=BPKBZ-UCICO-KTLWG-SP4TL-DF7TE-TMFDJ
    // https://apis.map.qq.com/ws/location/v1/ip?ip=61.135.17.68&key=BPKBZ-UCICO-KTLWG-SP4TL-DF7TE-TMFDJ
    var vm = new Vue({
        el: '#app',
        data: {
            info: ''
        },
        methods: {
            GetLocation() {
                console.log(this.info)
            }
        },
        mounted() {
            axios
            //这里会出错，因为访问api必须通过服务器进行，我们的demo是直接打开html格式文件，其实并没有架设本地服务器
                .get('https://apis.map.qq.com/ws/location/v1/ip?ip=61.135.17.686&key=BPKBZ-UCICO-KTLWG-SP4TL-DF7TE-TMFDJ')
                .then(response => (this.info = response))
                .catch(function(error) { // 请求失败处理
                    console.log(error);
                });
        }
    });
</script>
```

### 跨域问题
#### 1. 什么是跨域
跨域，指的是浏览器不能执行其他网站的脚本。它是由浏览器的同源策略造成的，是浏览器对JavaScript施加的安全限制。
#### 2. 什么是同源
同源指的是： $\color{#ee6600}{域名、协议、端口} $相同
> - https://www.douyu.com ==> https://www.huya.com ----- 跨域
> - https://www.douyu.com ==> https://www.douyu.com ----- 非跨域
> - https://www.douyu.com ==> https://www.douyu.com:8080 ----- 跨域
> - https://www.douyu.com ==> http://www.douyu.com ----- 跨域


#### 3. 如何解决跨域
1. 使用CORS（跨资源共享）解决跨域问题
> CORS是一个W3C标准，全称-“跨域资源共享”（Cross-Orign resource sharing）。它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。
> Cors需要浏览器和服务器同时支持。目前所有浏览器都支持该功能，IE浏览器不能低于IE10。整个CORS通信过程，浏览器自实现。对于开发者而言，CORS通信与同源的AJAX通信没有差别。浏览器一旦发现AJAX请求跨域，就会自动添加一些附加头信息，有时还会多出一次附加的请求，用户无感觉。
> 所以说，实现CORS的关键在于服务器实现CORS接口。 

2. 使用JSONP解决跨域问题
> JSONP(JSON with Padding)是JSON的一种“使用模式”，可用于解决主流浏览器的跨域数据访问的问题。由于是同源策略，一般来说位于```server1.example.com```的网页无法与```server2.example.com```的服务器沟通，而HTML的```<script>```元素是一个例外。利用```<script>```元素的开放策略，网页可以得到从其它来源动态产生的JSON资料，而这种使用模式就是所谓的JSONP。用JSONP抓到的资料并不是JSON，而是任意的JavaScript，用JavaScript直译器执行而不是用JSONP 解析器解析。

3. CORS和JSONP的比较
CORS和JSONP的使用目的相同，但是CORS比JSONP更强大。
JSONP只支持GET请求，CORS支持所有类型的HTTP请求。JSONP的优势在于支持老式浏览器，以及可以向不支持CORS的网站请求数据。

4. 使用Nginx反向代理解决跨域问题
以上跨域问题的解决方案都需要浏览器支持，当服务器无法设置```header```或提供```callback```时我们可以采取Nginx反向代理的方式解决跨域。
配置实例：在```Nginx.conf```的```location```中增加如下配置：
>  ```add_header Access-Control-Allow-Origin * 或域名 ```
> ```add_header Access-Control-Allow-Headers X-Requested-With; ```
> ```add_header Access-Control-Allow-Methods GET,POST,OPTIONS; ```



