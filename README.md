# 笔记

## 脚手架文件结构：
    |———— node_modules
    |———— public
    |     |——favicon.ico: 页签图标
    |     |__ index.html: 主页面
    |———— src
    |     |—— assets: 存放静态资源
    |         |__ logo.png
    |     |—— component: 存放组件
    |     |   |__ HelloWorld.vue
    |     |—— App.vue: 汇总所有组件
    |     |—— main.js: 入口文件
    |———— .gitnore: git版本管制忽略的配置
    |———— babel.config.js: babel的配置文件
    |———— package.json: 应用包配置文件
    |———— READEME.md: 应用描述文件
    |———— package-lock.json: 包版本控制文件

## 关于不同版本的Vue:
- Vue.js与Vue.runtime.xxx.js的区别：
      (1).vue.js是完整版的VUe，包含:核心功能+模板解析器
      (2).vue.runtime.xxx.js是运行版本的VUe，值包含：核心功能: 没有模板解析器。
- 因为vue.runtime.xxx.js没有模板解析，所以不能使用template配置项，需要使用
       render函数接受到的createElement函数去指定具体内容。

## vue.config.js配置文件
> 使用vue inspect > output.js可以查看到Vue脚手架的默认配置。
> 使用Vue.config,js可以对手脚架进行个性化定制，详情见https://cli.vuejs.org/zh

## ref属性
    1.被用来给元素或子组件注册引用信息（id的替代者）
    2.应该在html标签上获取真实的DOM元素，应用在组件标签上是组件实例对象（vc)
    3.使用方式：
        打标识：<h1 ref="xxx">....</h1> 或 <School ref="xxx"/>
        获取： this.$refs.xxx

## 配置项props
    功能：让组件接收外部传来的数据
        (1).传递数据：
            <Demo name:"xxx"/>
        (2).接收数据：
            第一种方式(只接收)：
                props：['name','age']

            第二种方式(限制类型)：
                props：{
                    name:Number
                }
            
            第三种方式(限制类型，限制必要性，指定默认值)：
                props：{
                    name:{
                        type:String //类型
                        required:true //必要性
                        default:'老王' //默认值
                    }
                }
        备注：props是只读的。Vue底层会检测你对props的修改，如果进行了修改，就会发出警告，
             若业务需求确实需要修改，那么请发至props的内容到data中一份，然后后去修改data中
             的数据

## mixin(混入)
        功能：可以吧多个组件公用的配置提取成一个混入对象
        使用方式：
            第一步定义混合，例如：
            {
                date(){....},
                methdos:{....},
                .....
            }
            第二步使用混入，例如：
                (1).全局混入：Vue.mixin(xxx)
                (2).局部混入：minxins:['xxx']

## 插件
    功能：用于增强Vue
    本质：包含install方法的一个对象，install的第一个参数是Vue，第二个以后的阐述是插件使用者传递的数据。
    定义插件：
        对象.install = function(Vue,Options){
            //1.添加全局过滤器
            Vue.fiilter(...)

            //2.添加全局指令
            Vue.directive(....)

            //3,配置全局混入(合)
            Vue.mixin(...)
            
            //4.添加实例方法
            Vue.prototype.$MYmETHOD = function(){...}
            Vue.prototype.$myProperty = xxx
        }
    使用插件：Vue.use()

## scoped样式
    作用:让样式在局部生效，防止冲突。
    写法:<style scoped>

## 总结TodoList案例

1.组件化编码流程

    (1).拆分静态组件：组件要按功能拆分，命名不要与html元素冲突。

    (2).实现动态组件：考虑好数据的岑芳位置，数据是一个组件在用，还是一些组件在用：

        1).一个组件在用：放在组件自身即可。

        2).一些组件在用：放在他们共同的父组件上()

    (3).实现交互：从绑定事件开始

2.props适用于：

    (1).父组件 ==> 子组件 通信
    (2).子组件 ==> 父组件通信 (要求父先给子一个函数）（状态提升）

3.使用v-model是要切记：v-model绑定的值不能是props传过来的值，因为props是不可以修改的！

4.props传过来的若是对象类型的值，修改对象中的属性石Vue不会报错，但不推荐这样做。

## WebStorage

1.存储的内容大小一般支持5MB左右（不同浏览器还可能不一样）

2.浏览器通过Window。sessonStorage 和 Window.localStorage 属性来实现本地存储机制

3.相关API：
    1.xxxxStroage.setItem('key','value');
        该方法接受一个键和值作为参数，会把键值对添加到存储汇总，如果键名存在，则更新器对应的值。
    2.xxxxStroage.getItem('person')
        该方法接受一个键名作为参数，返回键名对应的值。
    3.xxxxStroage.removeItem（'key')
        该方法接受一个键名作为参数，并把该键名从存储中删除。
    4.xxxStroage.clear()
        该方法会清空存储中的所有数据。
    
4.备注
    1.SessionStrage存储的内容会随着浏览器里窗口的关闭而消失
    2.LocalStroage存储的内容，需要手动清除才回消失
    3.xxxxStroage.getItem(xxx)如果xxx对应的value获取不到，那么getItem的返回值会是null
    4.JSON.parse（null）的结果依然是null

# 组件的自定义事件

1.一种组件间通信的的方式，适用于 子组件 ===> 父组件
2.使用场景：A是父组件，B是子组件，B想给A传数据，那么就要在A中给B绑定自定义事件（事件的回调在A中）
3.绑定自定义事件：
    1.第一种方式，在父组件中:<Demo @gatguigu="test"/> 或 <Demo v-on:atguigu="test"/>
    2.第二种方式，在父组件中：
        <Demo ref="Demo"/>
        ......
        mounted(){
            this,$ref.xxx.$on('atguigu',this.test)
        }
    3.若想让自定义事件只能触发一次，可以使用once修饰符，或者$once方法
4.触发自定义事件：this.$emit('atguigu',数据)
5.解除自定义事件：this.$off('atguigu')
6.组件上也可以绑定原生的DOM事件,需要使用native修饰符
7.注意：通过this.$refs.xxx.$on('atguigu',回调)绑定自定义事件时，回调要么配置在methods中，要么用箭头函数，否则this指向会出问题！

#全局时间总线（GlobalEventBus)
1.一种组件间的通信的方式，适用于任意组件间通信

2.安全全局时间总线：
 new Vue({
    ......
    beforeCreate(){
        Vue.prototype.$bus = this //安装全局事件总线,$bus就是当前应用的vm
    },
    ......
 })

 3.使用事件总线：
    1.接受数据：A组件想接受数据，则在A组件中给$bus绑定自定义事件，时间的回调则留在A组自身。
    methods(){
        demo(data){.....}
    }
    ....
    mounted(){
        this.$bus,$on('xxx',数据)
    }

4.最好在beforeDestory钩子中，用$off去解绑当前组件所用到的


# 消息定于与发布
1.一种组件间通信的方式，适用于任意组件间通信
2.使用步骤：
    1.安装pubsub：npm i pubsub-js
    2.引入：import pubsub from 'pubsub-js'
    3.接收数据：A组件想接收数据，则在A组件中订阅消息，订阅的回调留在A组件自身。
    methods(){
        demo(data){......}
    }
    ......
    mounted(){
        this.pid = pubsub.subscribe('xxx',this.demo)//订阅消息
    }
    4.提供数据：pubsub.publish('xxx',数据)
    5.最好在beforeDestory钩子中，用PubSub.unsubscribe(pid)去取消订阅

    # nextTick
    1.语法：this.$nextTick(回调函数)
    2.作用：在下一次DOM更新后结束执行其指定的回调。
    3.什么时候用：当改变数据后，要基于更新后的新的DOM进行某些操作时，要在nextTick所指定的回调函数中执行

    

#   V u e P r o j e c t s  
 