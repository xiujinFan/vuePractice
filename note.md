## Vue支持三元表达式
   <span>{{message?message:'hello'}}</span>
## 只绑定一次 视图变化的时候不再绑定
   {{*message}}
## 实例创建后，可以更改属性值，不能添加属性


1、后台MVC
      + M数据
      + V视图
      + C控制器
2、vue.js  MVVM框架
    数据可以影响视图，视图可以影响数据，双向绑定
3、体验vue.js
   1、模板渲染{{}}
   2、动态操作DOM属性 v-bind:title  :title="动态的数据"  :class (也是属性)
   3、条件判断 v-if
   4、事件 v-on:click   @click
   5、循环 v-for="(item,index) in ary"

###事件
- 基本事件：v-on：click（爆红，不影响使用）  @click
- 阻止冒泡和阻止默认事件
   - 阻止冒泡：
     +  e.stopPropagation?e.stopPropagation():e.cancleBubble=true
     +  @click.stop   vue提供的
   - 阻止默认事件
     + e.preventDefault?e.preventDefault():e.returnValue=true;
- 键盘事件
  - @keydown  @keyup   @keypress
  - 使用指定的键
    + @keydown.enter
    + @keydown.13
### 动态绑定样式
-  :class 非行间样式
   - {样式名：Boolean}  Boolean从data数据中拿到的
   - :class="obj"
   ```
   obj:{
         bg1:true,
         bg2:false
   }
   ```
-  :style 行间样式
    - {color:col1,background:col2}  col1、col2 都是变量
    ```
     data:{
       col1:"red",
       col2:"green"
     }
    ```
    - 可以简写为对象 :style="obj"

### 常用请求
- get请求
- post请求
- jsonp请求

bodyParser 用于解析客户端请求的body中的内容，内部使用JSON编码处理，url编码处理以及对于文件的上传处理

###防止闪烁，需要两步
- 在标签上添加 v-cloak
- 在style样式表中设置 [v-clocak]{display:none}

### 计算属性
- get
computed:{
  total(){
      return this.n*this.m
  }
}
- 如果想设置
computed:{
    total:{
         get(){
            return
         },
         set(newValue){
              //newValue就是新的total的值
         }
    }
}


- 实例上的一些常用方法
  + 拿到元素：app.$el
  + 拿到数据：
        + app.$data.属性名
        + app.属性名
  + 拿到自定义属性
        + app.$options.自定义的属性名
- 公用组件
  + 多个实例都能使用这个组件
- 基本组件封装涉及的知识
  + 注册组件：如果通过Vue.component 注册公用组件
  + 组件自己内部的数据
      data(){
         return {msg:123}
      }
  + 组件接受父组件传过来的数据，通过props属性
      + 动态属性操作 :n="name"  这个的name是个变量，它取决于父组件data中的值
      ```
      data:{
         name:xxx
      }
      ```
      + 接收的时候，子组件
          + props:['n']=>不限制传过来的数据类型
          + props:{n:String}=>限制传过来的必须是字符串类型
### 数据传递知识点
- 父组件通过props 给子组件传递数据  props:['n']
- 子组件通过事件发射给父组件传递数据  this.$emit
- 兄弟组件之间的数据传递  Event.$emit 进行发射；Event.$on进行绑定
- 数据同步，核心：传递对象，利用的是引用数据类型对地址的引用
- 数据不同步，核心：
   1）返回的必须是基本数据类型
   2）找一个临时变量接收父组件传过来的数据，更改的其实是临时变量的值

### vue-router
### 基本路由配置，5步
1、创建一个实例，并进行配置
```
  var router=new VueRouter({    //#home 即使template模板的id
      routes:[
          {path:'/home',component:Home}
      ]
  })
```
2、把配置好的router放入app实例中
```
 var app=new Vue({
    router,
    el:'#app'
 })
```
3、添加路由跳转
```
  //在link中写具体路由
  <router-link to="/home">首页</router-link>
```
4、显示组件
```
  <router-view></router-view>
``
5、设置默认路由`
```
  var router=new VueRouter({
     routes:[
       {path:'/home',component:Home},
       {path:'*',component:Home}
     ]
  })
```
### 子路由
1、需求：如果要在首页展示登录和注册按钮
    1）在Home组件的模板中设置如下：
    ```
     <router-link to='/home/login>登录</router-link>
     <router-link to='/home/reg'>注册</router-link>
     <router-view></router-view>
    ```
    2)到routes进行设置路由和模板
    ```
    var router=new VueRouter({
         routes:[
           {path:'/home',component:Home,
             children:[
              {path:'login',component:Login}  //这个接口 不能写 斜杠/
             ]
           },
           {path:'*',component:Home}
         ]
      })
    ```
### 路由参数
1、在router中设置模糊路由：'/list/news/:id'
2、在router-link中设置具体路由：'/list/news/1'
3、通过$route.params.id来获取路由参数
4、js中通过
```
  beforeEnter(to,from.next){
   //接收参数 to.params.id
   next();//下一步
  }
```