Vuex
1、npm i vuex -S
2、src-新建store -store.js
npm install vuex --save
store.js --- 
main.js 全局注册-Vuex

Vuex实现了一个单向数据流，在全局拥有一个State存放数据，-全局state
所有修改State的操作必须通过Mutation进行，
Mutation的同时提供了订阅者模式供外部插件调用获取State数据的更新。
所有异步接口需要走Action，常见于调用后端接口异步获取更新数据，而Action也是无法直接修改State的，
还是需要通过Mutation来修改State的数据。最后，根据State的变化，渲染到视图上。
Vuex运行依赖Vue内部数据双向绑定机制，需要new一个Vue对象来实现“响应式化”，所以Vuex是一个专门为Vue.js设计的状态管理库。

1.State-mutation
2.store  getters计算过滤操作
3.mapGetters
4.store actions异步修改状态  
5.actions是异步的改变state状态，而Mutations是同步改变

<!-- vue生命周期 -->
beforeCreate 创建之前
created 创建
beforeMount
mounted
//beforeMount、mounted 不会在服务端渲染过程调用，只会有操作数据来改变dom时调用
修改data最会在调用mounted改
beforeCreate、created 服务端渲染调用

beforeUpdate
updated
beforeDestroy
destroy
actived
deactived//组件

<!-- vue高级组件 -->
组件 -vue -parent 可以查看里面的配置，但是尽量不要去修改里面内容，会混乱逻辑？

//规范  组件定义的规范 prop定义规范
组件名需要大写

组件-props 单向数据流
注：props不要去修改数据
如果一定需要修改组件props的数据，props定义一个函数-绑定到组件上-通过点击事件通知
外面父组件-props需要改变-父组件就会通过方法去改变props值-绑定的值是动态的

子组件emit
父组件通过ref='comp1' 拿到实例

<!-- Vue组件的继承 -extend  -->
什么时候需要使用到extend
开发一个相对完善的组件，不需要那么功能，配置设置成默认值，并且扩展你的属性
const componet2 = {
  extends: compoent,
  data () {
    return {
      text: 1
    }
  },
  mounted () {
    console.log(this.$parent.$options.name)
  }
}

在父组件里使用extents 继承的子组件时，可以先打印console.log(this)  - this 看下继承子组件那些值，
如果 自定义组件compons2 data里定义了值和子组件一样的值时， 会覆盖子组件data定义的值，且需要通过this.$data.text 拿值
const componet2 = {
  extends: Comp,
  data () {
    return {
      
    }
  },
  mounted () {
    console.log('...............',this.$data.text)
  }
}

<!-- 网上总结--没理解什么意思 -->
子组件继承父母组件数据，请使用prop属性。
如果是想获得兄弟组件或者是子组件的数据，请使用Vuex，或者自己定义个全局变量



可以给我们组件指定一个 parent 
子组件可以通过parent 来拿到父组件data值

使用parent,可以查看里面东西，尽量不要去修改

<!-- vue组件-自定义双向绑定 -->
v-model model

<!-- Vue的组件高级属性 -->
slot 插槽
<slot></slot>
具名插槽
<slot name='body'></slot>
<span slot='body'></span>
作用域插槽
template1  <slot value='456' aaa='1111'></slot>
template2  <slot :value='value' aaa='1111'></slot>
data{
    value:'is value'
}

//可以调用模板数据，也可以调用本地数据
<span slot-scope="props">{{props.value}}</span>
//调用本地数据
<span slot-scope="props">{{props.value}} {{value}}</span>



//ref
<com-one ref="comp">
   <span ref='span'></span>
</com-one>
//可以通过$refs 取到组件数据-进行修改-但是不要这样用，一般用props
new Vue({
   mounted(){
    this.$refs.comp.value, 获取component
    this.$refs.span  获取span节点
   }
})

父子间-上下文关系子组件  childComponent 爷爷辈
爷爷辈 提供这个属性-provide
new Vue({
    provide:{
        yeye:this,
        value:this.value
    } -(改成function return {})
    provide(){
       return{
            yeye:this,
            value:this.value
       }
    }
})
孙子 通过inject 拿到爷爷辈属性
inject:['yeye','value']
mounted(){
    console.log(this.yeye,this.value)
}
//小问题 
如果爷爷绑定 <input v-model='value'/>
这个孙子也是绑定value
const ChildComponent = {
  template: '<div>child component: {{data.value}}</div>',
  inject: ['yeye', 'data'],
  mounted () {
    // console.log(this.yeye, this.value)
  }
} 时，这个爷爷输入input改变了孙子的 {{value}} 是没有发生改变的，解决办法 Object.defineProperty
provide () {
    const data = {}

    Object.defineProperty(data, 'value', {
      get: () => this.value,
      
      enumerable: true
    })

    return {
      yeye: this,
      data
    }
},


provide
inject

<!-- vue 组件 render function -->



<!-- Vue router 全家桶套餐 -->

路由渲染
src - config - routers.js、router.js

routers.js

router.js

router-view
-----------------------------
routers.js
//默认路由-主页
{
    path: '/',
    redirect: '/app'
},

//服务端渲染- URL #

router.js
加个
mode:'history' //去掉 #
base：'/base/' path+/base/
linkActiveClass:'' //class
linkExactActiveClass:''
//浏览器滚动行为
scrollBehavior(to , from ,savePosition){
    if(savePosition){
         return savePosition
    }else{
        return (x:0,y:0)
    }
}
fallback:true


<!-- 渲染后台数据是-取值 undefind -->


我们在code vue项目时一般会这样做:
  <div> {{obj.name.toString()}} </div>
但是当obj对象未定义或者不存在name的时候渲染DOM时会报错:obj.name is undefined;

这是因为name可能需要请求接口才能拿到,但是渲染DOM时接口可能还未拿到数据,

解决方法:

<div>{{obj.name && obj.name.toString()}}</div>

使用 && 逻辑可以先判断变量是否定义,如果定义再去执行 && 后边的逻辑,这样就可以避免页面渲染时报错;


<!--开发工具 -->
vue-devtools


<!-- 组件 -->
子组件通过props属性接收父组件传过来的值


vue父子组件传值之子传父：
通过自定义事件传递

子组件提交时this.$emit('自定义事件名', data)

父组件监听自定义事件获取子组件传递过来的data


 /* 
      路由的使用步骤：
      1 引入 路由插件的js文件
      2 创建几个组件
      3 通过 VueRouter 来创建一个路由的实例，并且在参数中配置好路由规则
      4 将 路由实例 与 Vue实例关联起来，通过 router 属性
      5 在页面中使用 router-link 来定义导航（a标签） 路由路口
      6 在页面中使用 router-view 来定义路由出口（路由内容展示在页面中的位置）
     */

// 配置路由规则
    const router = new VueRouter({
      // 通过 routes 来配置路由规则，值：数组
      routes: [
        // 数组中的每一项表示一个具体的路由规则
        // path 用来设置浏览器URL中的哈希值
        // componet 属性用来设置哈希值对应的组件
        { path: '/home', component: Home },
        { path: '/about', component: About },
        // redirect 重定向: 让当前匹配的 / ,跳转到 /home 对应的组件中, 也就是默认展示: home组件
        { path: '/', redirect: '/home' }
      ]
    })



<!-- 实际代码操作 -->
vue中点击获取当前对象

Vue.js可以传递$event对象，event.currentTarget对象就是当前元素

toggleList: function (event) {
  $("p.boxItem.selected").removeClass("selected");
  //获取点击对象
  var toggle = event.currentTarget;
  $(toggle).addClass("selected");
}








<!-- 响应式系统的基本原理 -->
observer、defineReactive、defineProperty、reactiveGetter 、reactiveSetter 

Object.defineProperty，
Vue.js就是基于它实现「响应式系统」的

function observer (value) {
    if (!value || (typeof value !== 'object')) {
        return;
    }
    
    Object.keys(value).forEach((key) => {
        defineReactive(value, key, value[key]);
    });
}

function cb (val) {
    console.log("视图更新啦～", val);
}

function defineReactive (obj, key, val) {
    Object.defineProperty(obj, key, {
        enumerable: true, /* 属性可枚举 */
        configurable: true, /* 属性可被修改或删除 */
        get: function reactiveGetter () {
            return val;         
        },
        set: function reactiveSetter (newVal) {
            if (newVal === val) return;
            cb(newVal);
        }
    });
}

class Vue {
     /* Vue构造类 *
    constructor(options) {
        this._data = options.data;
        observer(this._data);
    }
}

let o = new Vue({
    data: {
        test: "I am test."
    }
});
o._data.test = "hello,test.";

<!-- 响应式系统的依赖收集追踪原理 -->