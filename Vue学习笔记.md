---
title: Vue学习笔记
tags: 
   - 笔记 ##标签
   - 总结
categories: 学习 ##分类
toc: true #是否显示文章目录
author: youyo
abbrlink: 57528
---
#### 1. 为什么要用vue
1. 数据与视图分离
2. 性能更高，减少了dom操作（diff算法）
3. 组件化开发（每个组件都有自己的作用域）

vue全家桶：vue vuex vue-router axios elementui/antd webpack node npm es6 vue/di
   
#### bootStrap提醒老师讲

#### 2. 差值表达式
双大括号`{{}}`提供了一个js执行环境，可以写任意的js表达式，但是不识别html结构

#### 3. 指令
**指令都是以`v-`开头的，实现特定的功能，写在元素的开始标签中**
**注意：**所有的指令后面的引号`""`是提供一个js执行环境，不是字符串的引号，要想在里面写字符串需要双引号里嵌套单引号
1. v-html="数据" 把一段html结构渲染到他所绑定的元素中
2. v-text 和双大扩号作用差不多，不识别html标签
3. v-bind:属性="数据(js表达式)" 可简写为 `:属性="数据(js表达式)"`
   + 特殊情况:class
        1. 可以绑定一个字符串，字符串名就是class名
        2. 可以绑定一个对象，对象的属性名就是class名，对象的属性值是布尔值，代表是否有这个class名
        3. 可以绑定一个数组，引号里是数组的变量名，变量值就是class名

    + 特殊情况: style
        1. 绑定一个对象，对象就是样式对象
        2. 绑定一个数组，就是绑定多个样式对象
    ```js
        <div id="weather" :class="msg" :style="style2">{{msg}}</div>
        <script>
            new Vue({
                el:"#weather",
                data:{
                    //class名
                    msg:["1","2","3"],
                    msg2:[
                        {class1: false},
                        {class2: true}
                    ],
                    msg3:{classn:true},
                    //style样式
                    style1:{
                        width:"100px",
                        height:"200px",
                        background:"red"
                    },
                    style2:[
                        {width:"300px",height:"300px"},
                        {background:"yellow"}
                    ]
                }
            })
        </script>
    ```
4. v-on:事件名="函数名" 缩写成@事件名="函数" **注意:**如果函数没有参数不能在后面加括号`fn()`错误，有参数才能加`fn(n)`正确
5. v-if v-else-if v-else 真正的条件渲染(真正删除了元素)，只能做显示隐藏**尽量避免和v-for一起使用，因为v-for的优先级是大于v-if的,所以循环几次v-if就会使用几次**
6. v-show 条件渲染，只是基于样式的切换(将display属性改变)
   **区别**:v-show有更高的初始渲染消耗，v-if有更高的切换消耗
7. v-for="item in array" 列表渲染（动态循环创建元素）
   + 可以循环一个数组，item代表每一项  参数有(item index)
   + 可以循环一个对象, item代表每一项的value 参数有(value key index)**第一个参数不用直接写null,一般不会有这种情况**
   + **注意**:数组是不能直接通过角标和length进行修改，这些都不是响应式的
   + 对于对象来说，如果添加**本来不存在**的属性，那么该属性也不是响应式的
```js
    1. 如果需要修改，选择数组自带的方法，或者使用Vue.set(要修改的数据,角标,新值)，也可以对数组进行重新赋值（进行全体换）
    2. 如果需要添加对象属性, Vue.set(要修改的对象,属性名,属性值)（一般情况是不会删除对象属性的）
```

8. v-model:表单标签的双向绑定
   + v-model默认绑定到表单的value属性
   + 多选框：单个多选框，绑定到布尔值，多个复选框，绑定的是数组
   + 单选框：值直接绑定到value值
   + 下拉选择框：直接绑定到值（给select加v-model）、

#### 4. methods
methods里面写所有的函数，**如果在模板中调用methods里面的函数的话，改变其他与之无关的值，会导致该函数再次执行**，可以用计算属性来解决

#### 5. computed：计算属性
**将逻辑运算写在这里面，避免在模板里面写过多的js逻辑**
+ 计算属性是一个函数，一定要return一个值
+ 计算属性里面的函数一定是它所依赖的值发生变化才会去执行的（不主动调用）
+ 性能：计算属性的性能更高
+ **计算属性的属性名（函数名）不能和data里面的变量名一样，计算属性可以同时处理多个数据的变化**
**面试题？？？？**计算属性和函数的区别：

#### 6. watch:监听器
**用来监听一个数据，只要数据发生变化，就会执行一个函数**
要监听的数据（data或者computed里面的）名就是函数名，
**面试题？？？？**监听器和计算属性的区别：监听器是干什么事情的，计算属性是返回一个值的。

#### 7. 验证为什么v-for循环需要key案例
```js
     <div id = "app">
        <input type="text" v-if="see"/>
        <input type="password" v-else/>
        <button @click="change">更改</button>
    </div>
    <script>

        new Vue({
            el:"#app",  // 一个Vue实例对应一个根容器
            data:{
                see: true,
            },
            methods:{
                change(){
                    this.see=false
                }
            }
        })
    </script>
```
上面这个案例有两个input输入框，不同点只是input框的type属性不一样，使用v-if和 v-else命令对输入框进行条件渲染，当点击更改按钮时，原来text输入框的值却在后面显示的password输入框中以*号显示。**原因：Vue使用diff算法，在Vue中会尽量的复用dom，Vue会维护一个虚拟的dom。上面的例子Vue其实没有重新生成新的dom而是借助原来的text属性的input框，将这个输入框的属性改一下（type属性）继续复用，而value值不变，所以会发生这中情况**，要想让Vue生成新的dom元素，需要在标签中绑定`:key="（字符串）"`属性,这个key值应该是独一无二的。

#### 8. diff算法（自行了解）

#### 9. 事件修饰符
有很多，还有按键修饰符（按键码）,`v-model.trim`自动去首尾的空格(不管有没有要求，自己注意加上)。事件修饰符可以连续调用。

#### 10. 组件
1. 组件的注册
   + 全局注册：
    ```js
        Vue.component("组件名",{
            template:"html代码块",
            data(){

            },
            //其他的和Vue实例完全一样
        })
    ```
    **注意**: data必须是一个函数,template只有一个根节点
    + 局部注册:(1.先定义组件的配置选项2.在要使用的组件中去注册)
    ```js
        var head = {
            template: `<div><h1>{{msg}}</h1></div>`,
            data(){
                return {
                    msg: "我是头部"
                }
            }
        }

        new Vue({
            el:"app",
            data:{

            },
            components:{  //因为要在根组件里面使用，所以在这里注册
                myhead:head, 
            }
        })

    ```

2. 组件之间数据传递
    + 父-->子 props
    (1.子组件定义props 2.子组件规定该属性用在哪里 3.组件调用，在开始标签里面传递来自父组件的数据)**props可以是数组，也可以是对象{属性名:类型}**
    ```js
        props: {
            // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
            propA: Number,
            // 多个可能的类型
            propB: [String, Number],
            // 必填的字符串
            propC: {
                type: String,
                required: true
            },
            // 带有默认值的数字
            propD: {
                type: Number,
                default: 100
            },
        }
    ```
    ```js
        <div id="app">
            <mynav :color="style[0]"></mynav>
            <mynav :color="style[1]"></mynav>
            <mynav :color="style[2]"></mynav>
            <mynav :color="style[3]"></mynav>
        </div>
        <script>
            const mynav = {
                template:`<ul :style="color" style="width: 200px; list-style: none;">
                            <li v-for="(item,index) in navArr" :key="index"  style="margin-right: 10px; float: left; background: #5696ff">{{item}}</li>
                            <div style="clear: both;"></div>
                        </ul>`,
                props:["color"],
                data(){
                    return {
                        navArr:["首页","内容","游戏","资讯"],
                    }
                },
            }

            new Vue({
                el:"#app",
                data:{
                    style:[
                        {color:"red"},
                        {color:"yellow"},
                        {color:"blue"},
                        {color:"white"}
                    ]
                },
                components:{
                    mynav,
                }
            })
        </script>
    ```
    + 跨级(爷爷传数据给孙子)传输数据（只能一级一级传输，不能直接跨级）
    ```js
        <div id="app">
            <father :fmsg="msg"></father>
        </div>
        <script>
            const son = {
                        template:`<h3>{{smsg}}</h3>`,
                        props:["smsg"],
                    }
            const father = {
                template:`<div style="background:#5696ff;width:200px;color:white;">
                            <son :smsg="fmsg"></son>
                        </div>`,
                props:["fmsg"],
                components:{
                    son,
                }
            }
            new Vue({
                el:"#app",
                data:{
                    msg:"爷爷的信"
                },
                components:{
                    father,
                }
            })
        </script>
    ```
    + 子-->父 自定义时间$emit **面试题？？？？**
    1. 在子组件开始标签绑定一个事件
    2. 在该子组件内部定义方法，该方法要通过`$emit`去触发自定义事件，并且把自己的数据作为`$emit`的第二个参数传递进去
    3. 自定义事件要做的事定义在父组件methods中，在此方法里只需要把自己的数据更新为传进来的参数即可
    
    ```js
        
        <body>
            <div id = "app">
                {{msg}}
                <mylist @myevent="achange"></mylist>
            </div>
        </body>
        <script>
            //1. 在子组件模板中里绑定普通事件，通过普通事件触发$emit方法创建自定义事件并传入第二个参数（要传给父级的数据）
            //2. 在父组件的模板中绑定自定义事件，自定义事件触发的函数就是父组件的methods里的函数，函数里的形参就是子组件传来的数据

            const mylist = {
                template:`<div @click="change">我是子组件</div>`,
                data(){
                    return {
                    mymsg:"我是子组件"
                }
                },
                methods:{
                change(){
                    this.$emit("myevent",this.mymsg)
                }
                }
            }
            new Vue({
                el:"#app",
                data:{
                    msg:"我是父组件"
                },
                methods:{
                    achange(data){
                        this.msg = data
                    }
                },
                components:{
                    mylist:mylist,
                }
            })
        </script>
    ```
    
#### 11. 插槽
组件标签中的内容会渲染到插槽的位置上
具名插槽，没有名字的插槽默认名字为default
1. `<slot name="名字"></slot>`
2. template标签包裹
    ```js
        <template v-solt:名字>
            <h1>这里是文字</h1>
        <template>
    ```
3. v-slot可以缩写为#
4. 如果模板中想要使用该组件内部的数据，首先需要在模板的插槽中定义v-bind:属性="数据" 组件调用的地方插槽通过v-slot:插槽名="prop" prop代表所有属性的集合 取得数据prop.属性
5. 例子：
    ```js
        <div id = "app">
            <mylist>
                <div>
                    <h2>没用的文字</h2>
                </div>
                <!-- 标签想要插入到具名插槽必须要用template标签包裹，因为v-slot指令只能作用于template标签-->
                <!--  second 插槽名字 -->
                <template v-slot:second="prop">  
                    <p>second</p>           
                    <!-- <span>{{mymsg}}</span>不能直接访问即将插入的组件里的数据，只能用该模板对应组件里的数据-->
                    <span>{{msg}}</span>   
                    <!-- 访问即将插入的组件里的数据另有方法： -->
                    <span>{{prop.aaa}}</span>   
                    <span>{{prop.data}}</span>
                </template>
                <!-- 每一个不同名插槽的prop属性集合都是独立的互不影响 -->
                <template #first="prop">   
                    <p>first</p>
                    <span>{{prop.aaa}}</span>
                </template>
                <h3>插入默认(名称default)插槽</h3>
            </mylist>
        </div>
        <script>
            const mylist = {
                template:`<div >
                        <option >1</option>
                        <slot name="first" :aaa="data"></slot>
                        <option >2</option>
                        <slot></slot>
                        <option >3</option>
                        <slot name="second"  :aaa="mymsg" :data="data"></slot>   
                        <option >4</option>
                    </div>`,
                data(){
                    return {
                    mymsg:"我是子组件",
                    data:"这里是数据"
                    }
                },
            }
            new Vue({
                el:"#app",
                data:{
                    msg:"我是父组件"
                },
                components:{
                    mylist:mylist,
                }
            })
        </script>
    ```
6. 使用的场景：封装弹框组件，封装下拉框（可插入不同内容的组件）

#### 12. 组件的生命周期
1. 就是在某个时间会自动触发的事件（是函数）
    ```js
        beforeCreate() {
            console.log("组件创建之前")
        },
        created() {
            console.log("组件创建完毕")
        },
        beforeMount() {
            console.log("组件渲染前")
        },
        mounted() {
            console.log("组件挂载完毕")
        },
        beforeUpdate() {
            console.log("组件更新前触发")
        },
        updated() {
            console.log("组件更新后")
        },
        beforeDestroy() {
            console.log("组件销毁前")
        },
        destroyed() {
            console.log("组件销毁后") //组件销毁让定时器停掉，内存释放
        },
    ```

#### 13. 动态组件
1. `<component v-bind:is="组件名"></component>`
2. 例子
   ```js
        <div id="app">
            <component v-bind:is="title"></component>
            <button v-on:click="change(2)">按钮</button>
        </div>
        <script>
            var coma = {
                template: `<div><h1>我是第一个组件</h1></div>`,
            }
            var coma2 = {
                template: `<div><h1>我是第二个组件</h1></div>`,
            }
            var coma3 = {
                template: `<div><h1>我是第三个组件</h1></div>`,
            }
            var app = new Vue({
                el: "#app",
                data: {
                    tab: "mycom",
                    title: "mycom"
                },
                components: {
                    mycom: coma,
                    mycom2: coma2,
                    mycom3: coma3,
                },
                methods: {
                    change(n) {
                        this.title = "mycom" + n
                    }
                }
            })
        </script>
   ```

#### 14. vue/cli脚手架
1. 安装
   + node
   + npm:node package manager
   + `npm init` ：引导完成项目基本的配置信息,生成package.json文件，里面有各种依赖包的信息
   + `npm install`：安装package.json文件中的依赖包到并且生成node_modules文件夹
2. 搭建vue脚手架：
   + npm install -g @vue/cli   （-g全局安装vue脚手架）
   + vue create 项目名称
   + 配置
   + 补充创建vue项目步骤：
     + npm install 包名 [--save] (--save可不加)（安装包时默认配置到生产依赖里面）
     + npm install 包名 --save-dev (安装包时默认配置到开发依赖里面)  
3. 创建项目选择设置步骤（vue create 项目名）
   1.  ？Please pick a preset **选择 Manually select features（手动选择功能）**
   2.  ？Check the features needed for your project:
       + (*) Choose Vue version **选择以下这些**
       + (*) Babel
       + (*) Router
       + (*) Vuex
       + (*) CSS Pre-processors
       + (*) Linter / Formatter
    3.  ？Choose a version of Vue.js that you want to start the project with  **选择 2.x**
    4.  ？Use history mode for router? **输入n（不使用history路由模式）**
    5.  ？Pick a CSS pre-processor  **选择 less**
    6.  ？Pick a linter / formatter config: **选择 ESLint with error prevention only**
    7.  ？ Pick additional lint features: **选择 Lint on save**
    8.  ？Where do you prefer placing config for Babel, ESLint, etc.  **选择 In package.json**
    9.  ？Save this as a preset for future projects? **输入n （意思是不要保存这次预设作为以后项目创建的预设）**
4. 脚手架生成文件的说明
   + **node_modules**：用于存放我们项目的各种依赖，比如axios等等，没有modules文件，项目就无法运行，可以使用npm install 进行项目依赖的安装，会自动安装package.json中配置的依赖
   + **public**：用于存放公共的静态文件
   + **public/index.html**：是一个模板文件，作用是生成项目的入口文件，webpack打包的js，css也会自动注入到该页面，浏览器访问的项目的时候就会默认打开生成好的index.html
   + **src**：我们存放各种vue文件的地方
   + **src/assets**：用于存放给中静态文件，（图片字体库等）
   + **src/components**：用于存放我们的公共组件，如header、footer等
   + **src/views**：用于存放我们写好的各种页面，如login、main等单文件组件页面
   + **src/App.vue**：主vue模块引入其他模板，App.vue是项目的主组件，所有的页面都是在App.vue下切换的
   + **src/main.js**：入口文件，主要所用是初始化vue实例，同时可以在此文件中引用某些组件库，或者全局挂一些变量
   + **src/router.js**：路由文件，配置各个页面的地址路径，用于访问，同时可以直接在里面编写路由守卫
   + **src/store.js**：主要用于项目里边的一些状态的保存，state中保存状态，mutations中写用于修改state中的状态，actions暂时没实践，不知道具体怎么使用
   + **package.json**：模块的基本信息，项目开发所需要的模块，版本，项目名称等
   + **package-lock.json**：是npm install时生成的一份文件，用来记录当前状态下实际安装的各个npm package的具体来源和版本号
   + **babel.config.js**：是一个工具链，主要用于在当前和较旧的浏览器或环境中将ECMAScript2015+代码转换为JavaScript的向后兼容版本
   + **gitignore.git**：上传需要忽略的文件格式
   + **vue.config.js**：保存vue的配置文件，可以用于设置代理，打包配置等
   + **README.md**：项目简单的使用说明
   + 补充:
        + **src/utils**: 写一些封装的插件工具代码
        + **src/common**: 存放公共样式
        + **spa**: single page application 单页面应用(**优点**:切换特别流畅),vue适合写单页面应用,比较适合做移动端
        + 解析vue2.0页面渲染过程? **面试题?????**
        + 引入包
        ```js
            import router from './router'  
            import router from 'axios'
            //from 后面带斜杆'/'表示路径,不带斜杠表示直接引入核心模块(node_modules)里的包
        ```
5. 单文件组件：以.vue为后缀的文件.
   + 包含html css js的一个页面组件，html写在`<template>`标签中（必须保证只能有一个跟节点dom）
   + js写在`<script>`标签里
    ```js
        export default{
            data(){
                return {}
            },
            methods:{}
            components:{
                HelloWorld
            }
            ...
        }
    ```
    + css写在`<style>`,style里面可以添加`scoped`，代表该样式仅仅在当前组件内生效
6. 组件渲染原理
   ```js
        new Vue({
            router,
            store,
            render: h => h(App)
        }).$mount('#app') //.$mount('#app')主动挂载到dom上
   ```
   等同于：render:function(h)=>{return h(App)}
   等同于：render:function(createElement){return createElement(App)}
7. @代表src
8. 杂项:
    + cli中自带了webpack会自动解析vue文件
    + vscode安装vue vscode snippets插件后 直接在.vue文件里输入`vbase-css` 自动生成但组件默认结构

#### 15. 路由
单独安装路由 `npm install vue-router`
1. vue引入的所有插件都需要注册Vue.use(插件名) **.use()原理???**
2. `router`: 跟实例路由
3. `route`: 每个组件自己的路由
    ```js
        new VueRouter({
            //接受路由配置参数
        })
    ```
4. 路由配置项: 规定路径和组件的对应关系
   ```js
    import Vue from 'vue'
    import VueRouter from 'vue-router'
    Vue.use(VueRouter)
    const routes = [
    {
        path: '/',
        name: 'Home',                       
        component: () => import('@/views/Home')  //@为项目的src目录，home不加后缀默认找home.vue文件.
        //此处引入路由是懒加载的方式
    },
    {
        path: '/details',
        name: 'Details',
        component: () => import('@/views/Details')
    }
    ]
    const router = new VueRouter({
        routes
    })
    export default router
   ```
    + 子路由path配置如果不加斜线`/`那么默认在上一级路由path上叠加,如果加了斜线`/`那么前面必须带上上级路由path.
    + **路由懒加载**: 提高性能 `component: ()=>import('../views/About.vue')
5. 引入路径的默认写法
   + `import Abc from "./demo"` 引进来的路由名称首字母要大写.**以下是文件引入搜索顺序**:
   1. 如果当前目录下都有demo文件夹,那么首先会去引入demo文件下的index.js文件
   2. 如果不存在demo文件夹,会去找当前目录下的同名.vue文件(demo.vue文件)
   3. 如果不存在同名.vue文件(demo.vue文件),会优先查找当前目录下的同名.js文件(demo.js文件)
6. 使用路由的步骤
    1. 定义路由组件(单文件组件) 
    2. 定义路由配置项()   1和2 一般在一般是views文件夹下的.vue文件
    3. 创建路由实例,传入配置项 一般在router文件夹下的index.js文件里
    4. 挂载 在main.js文件中
7. 路由的视图显示:路由必须要有router-view来规定路由的显示位置
   `<router-view></router-view>`
8. 嵌套路由:哪个组件还有子路由,那么需要在该组件内部(模板里)定义router-view,代表该组件的子路由显示在该位置上.
9. $route: 每个组件自己的路由对象
    ```js
        {
            name:"组件的名字",
            fullPath:"完整路径",
            path:"路径",
            matched：[匹配上的所有路由],
            meta:"路由元信息",
            params:"动态路径参数",
            query:"查询参数"
        }
    ```
10. 路由的跳转 
    1. 通过router-link 标签(其实就是vue封装的a标签)
    ```html
        <router-link to="/about"></router-link>
        <!-- to属性后面的值可以是字符串路径,也可以是对象{path:"路径"}或者{name:"路由的名字"}} -->
    ```
    2. 通过js跳转:
    ```js
        this.$router.push(字符串路径)
        this.$router.push({path:"home"})
        this.$router.push({name:"user"})
        //用法和标签<router-link>中的to属性一样
    ```
11. 动态路由匹配 在路由配置项中的path后面加`/:名字` 传入的参数在route里的params中
    ```js   
        {   //动态路径参数 以冒号开头
            path: '/details/:id',
            name: 'Details',
            component: () => import('@/views/Details')
        }
    ```
12. 查询参数 query 与get方式请求传参一样,在地址栏用?键=值&键=值方式
13. params和query的区别
    ```js
        <router-link :to="{name:'user', params:{userId: 123}}">User</router-link>
    ```
    + 如果用的path跳转,那么只能使用query.如果用的name跳转,只能使用params
14. 路由的重定向:
    1. 重定向: 当用户访问/a时,url将会被替换成/b,然后匹配路由/b
    2. redirect:"要重定向的路径"
    ```js
        {
            path:'/a',
            redirect:'/b'
        }
        //重定向的目标也可以是命名的路由
        {
            path:'/a',
            redirect:{name:'details'}
        }
        //或者一个方法,动态返回重定向目标:
        {
            path:'a',
            redirect: to => {
                //方法接收 目标路由  作为参数
                //return 重定向的 字符串路径/路劲对象
            }
        }

    ```
15. 路由别名:**不受父路由限制**
    + 别名: /a的别名是/b意味着,当用户访问/b时,url会保持为/b,但是路由匹配则为/a,就像用户访问/a一样.对应的路由配置
    ```js
        {
            path:'/a',
            component: A,
            alias:'/b'
        }
    ```
16. 路由模式**后面再完善???** **如何解决history模式404问题 面试题????**配置nginx
    1. hash模式  路径中有/#,不好看,(改变地址栏不会发送请求)
    2. history模式  没有/#,但是需要后端配置支持,(改变地址栏会发送完整的路由请求,会有服务器请求)
17. 编程式导航跳转(js跳转)
    ```js
        //字符串
        router.push('home')
        //对象
        router.push({path:'home'})
        //命名的路由+传参
        router.path({name:'user',params:{userId:'123'}})
        //带参数查询,变成/register?plan=private
        router.path({path:'register'},query:{plan: 'private'})
    ```
18. 杂项:
    +  **`$`表示vue根实例里暴露的属性或者方法(也就是new Vue({})里面的属性或者方法)**,`$this.route`表示当前路由实例,`$this.router`表示根实例
    +  动态路由组件里不能有子路由组件
    +  引入axios时:
    ```js
        import axios from 'axios'
        Vue.prototype.$axios = axios;
        //将axios对象添加到vue的原型对象中,这里的$axios只是普通变量,因为所有的根实例里暴露的属性或方法命名时都约定成俗的在变量名前加个$符号,这便于与用户定义的属性区分开来
    ```
    + 同一级的多个路由不能同时显示
#### 16. 导航守卫
1. 全局前置守卫:页面中**任何路由**跳转之前要做的事
   + 使用`router.beforeEach`注册一个全局前置守卫:
    ```js
        const router = new VueRouter({...})
        router.beforeEach((to, from, next) => {
            //...
        })
    ```
   + 当一个导航触发时,全局前置守卫按照创建顺序调用.守卫是异步解析执行,此时导航在所有守卫resolve完之前一直处于**等待中**
   + 每个守卫方法接受三个参数:
        1. `to:Route`: 即将要进入的目标**路由对象**
        2. `from:Route`: 当前导航正要离开的路由
        3. `next:Function`:一定要调用该方法来resolve这个钩子,实行效果依赖`next`方法的调用参数 
            + `next()`:进行管道中的下一个钩子,如果全部钩子执行完了,则导航的状态就是confirmed(确认的) 只有不带参数的next()方法才是放行
            + `next(false)`:终端当前的导航,如果浏览器的URL改变了(可能是用户手动或者浏览器后退按钮),那么URL地址会重置到`from`路由对应的路由
            + `next('/')`或者`next({path'/'})`:跳转到一个不同的地址,当前的导航被中断,然后进行一个新的导航,你可以向`next`船体任意位置对象,且允许设置诸如`replace:true`,`name:'home'`之类的选项以及任何用在`router-link`的`to` `prop` 或者`router.push`中的选项
            + `next(error)`:(2.4.0+)如果传入`next`的参数是一个Error实例,则导航会被终止且该错误会被传递给`router.onError()`注册过的回调
            + **next大坑???** Vue13
            ```js
                router.beforeEach((to,from,next)=>{
                    if(sessionStorage.getItem('loginData')){
                        Toast('跳转成功')
                        next();
                    }else{
                        next({path:'/login'}) //守卫导航到login但并没有放行
                    }
                })
            ```
                加个验证ok!
            ```js
                router.beforeEach((to,from,next)=>{
                    //console.log(to);
                    //console.log(from);
                    if(sessionStorage.getItem('loginData')){
                        Toast('跳转成功')
                        next();
                    }else(){
                        //没有登录,去跳转到登录页面
                        if(to.path === '/login'){
                            next()
                        }else(){
                            next({path:'/login'})
                        }
                    }
                })
            ```
2. 全局解析守卫