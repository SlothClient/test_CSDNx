
# vue3基础

## 创建vue3工程

```bash
# vue2基于CLI脚手架创建项目，vue3基于vite创建，更加高效
npm create vue@latest
# 选择需要的功能
# 输入项目名[project_name]
cd [project_name]
# 尽量使用npm命令，迫不得以再使用cnpm等国内源
npm install
npm run dev
# 复制Local链接打开即可
```

### 项目结构分析

1..vscode文件夹->开发工具配置文件夹

2.node_modules文件夹->项目依赖包

3.public文件夹->公共资源文件夹

4.src文件夹->项目源码文件夹

5..gitignore文件->git忽略文件

6.index.html文件->项目入口文件

7.package.json文件->项目配置(信息描述)文件（其中的vite是与webpack类似的脚手架，可以快速搭建项目）

vite.config.js文件->Vue配置文件

### 缺少node_modules报红

执行`npm i`安装项目需要的依赖解决

## 编写基础代码

### index.html入口

### main.js

```js
// 容器
import {createApp} from 'vue'
// 根组件
import App from "./App.vue"
// 创建实例
const app = createApp(App)
// 挂载对象
app.mount("#app")
```

### App.vue

```vue
<template>
	<div class="hello">
        hello world!
        <Component/>
    </div>
</template>
<script>
    import Component from "./components/component.vue"
	exports default{
    // 注意此处为vue2选项式写法
        name:"App",
        data(){
            
        },
        methods:{
            
        }
    }
</script>
<style>
    .hello {
        
    }
</style>
```

### 文件夹components→Component.vue

```vue
<template>
	<div class="intro">
        my name is lucy
    </div>
</template>
<script>
	exports default{
    // 注意此处为vue2选项式写法
        name:"Component",
        data(){
            
        },
        methods:{
            
        }
    }
</script>
<style>
    .intro {
        
    }
</style>
```

### vue3组合式写法

```vue
<!-- vue2 -->
<template>
    <div class="hello">
        {{ msg }}
        <button @click="sayHello">打招呼</button>
        <intro/>
    </div>
</template>
<script>
    import intro from './components/intro.vue';
    export default {
        name: 'App',
        // components注册组件
        components: {
            intro
        },
        setup(){
            // 当前变量为非响应式
            let msg = 'hello vue3';
            function sayHello(){
                alert('hello vue3');
            }
            /**
             * 相当于
            return {
                msg:msg,
                sayHello:sayHello
            }
             */
            return {
                msg,
                sayHello
            }
        }
    }
</script>
<!-- scoped单文件生效 -->
<style scoped>
    .hello {
        width: 100%;
        height: 500px;
        background-color: #f9f;
        border-radius: 10px;
        box-shadow: 3px 5px 2px 2px rgba(0, 0, 0, .6);
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
    }
</style>
```

#### 注意点

1. vue2写法和vue3写法可以共存

2. vue3的setup()不能使用this获得当前vue对象

3. vue3的setup()函数比生命钩子函数beforeCreate()先执行

4. vue2的选项式可以使用setup()函数中的成员，直接使用this调用

   > 想来应该是执行完返回值交给了vue对象，并不是说能和data中的数据、methods中的函数并列

​	5.vue3的setup()不能使用选项式中的对象

### setup()语法糖

```vue
<script>
    import intro from './components/intro.vue';
    export default {
        name: 'App',
        // components注册组件
        components: {
            intro
        }
    }
</script>
<!-- 多加一个script但省去了setup()函数的声明和繁琐的值返回 -->
<script setup>
	// 当前变量为非响应式
    let msg = 'hello vue3';
    function sayHello(){
        alert('hello vue3');
    }
</script>
```

借助插件还可以合并为一个script

步骤如下

> 终端执行npm i vite-plugin-vue-setup-extend -D
>
> 找到vite.config.js先引入插件，再在plugins中调用即可
>
> *import vueSetupExtend from 'vite-plugin-vue-setup-extend*
>
> *plugins: [*
>
> *vue(),*
>
> *vueSetupExtend()*
>
> *]*

```vue
<script setup name="App030">
    import intro from './components/intro.vue';
    components: {
        intro
    }
	// 当前变量为非响应式
    let msg = 'hello vue3';
    function sayHello(){
        alert('hello vue3');
    }
</script>
```

## 响应式

### 基本类型的响应式数据

```vue
<script setup name="Introduce">
    // 注意别忘了引入ref
    import { ref } from "vue"
    // 注意响应式数据，需要使用ref包裹
    let name = ref("张三")
    let gender = ref("男")
    let age = ref(18)
    let phone = ref("13500000000")
    let email = ref("<EMAIL>")
    let changeName = () => {
        // 注意，修改响应式数据，需要使用value
        name.value = "李四"
    }
    let changeAge = () => {
        age.value++
    }
</script>
```

### 对象类型的响应式数据

#### ref:refImpl对象

```vue
<script setup name="Introduce">
    // 注意别忘了引入reactive和ref
    import { ref,reactive } from "vue"
    /**
     * 注意响应式数据，
     * 使用reactive包裹，底层生成了一个代理对象
     * 使用ref包裹，底层生成了一个refImpl对象
     */
    // let shoppingCart = reactive([
    //     {id:"sp001",name:"sp001",price:100,count:2},
    //     {id:"sp002",name:"sp002",price:150,count:3},
    //     {id:"sp003",name:"sp003",price:250,count:4}
    // ])
    let shoppingCart = ref([
        {id:"sp001",name:"sp001",price:100,count:2},
        {id:"sp002",name:"sp002",price:150,count:3},
        {id:"sp003",name:"sp003",price:250,count:4}
    ])
    let sourceCart = reactive([
        {id:"sp001",name:"sp001",price:88,count:2},
        {id:"sp002",name:"sp002",price:121,count:3},
        {id:"sp003",name:"sp003",price:222,count:6}
    ])
    // // reactive包裹的对象
    // let addCount = (index)=>{
    //     shoppingCart[index].count++
    // }
    // let alignCart = (index)=>{
    // 对象一整个指到其他地方去了，原来的响应并没有失去=>你搬家了，房子不是你的了，它不管怎么装修，与你无关
    //     shoppingCart = sourceCart
    //     console.log(shoppingCart);
    // }

    // ref包裹的对象
    let addCount = (index)=>{
        shoppingCart.value[index].count++
    }
    let alignCart = (index)=>{
        // 对象一直没变，只是内含的东西变了=>你长大了，你还是你
        shoppingCart.value = sourceCart
        console.log(shoppingCart);
    }
</script>
```



#### reactive:proxy对象

```vue
<script setup name="Introduce">
    // 注意别忘了引入reactive和ref
    import { ref,reactive } from "vue"
    /**
     * 注意响应式数据，
     * 使用reactive包裹，底层生成了一个代理对象
     * 使用ref包裹，底层生成了一个refImpl对象
     */
    let shoppingCart = reactive([
        {id:"sp001",name:"sp001",price:100,count:2},
        {id:"sp002",name:"sp002",price:150,count:3},
        {id:"sp003",name:"sp003",price:250,count:4}
    ])
    // let shoppingCart = ref([
    //     {id:"sp001",name:"sp001",price:100,count:2},
    //     {id:"sp002",name:"sp002",price:150,count:3},
    //     {id:"sp003",name:"sp003",price:250,count:4}
    // ])
    let sourceCart = reactive([
        {id:"sp001",name:"sp001",price:88,count:2},
        {id:"sp002",name:"sp002",price:121,count:3},
        {id:"sp003",name:"sp003",price:222,count:6}
    ])
    // reactive包裹的对象
    let addCount = (index)=>{
        shoppingCart[index].count++
    }
    let alignCart = (index)=>{
         // 对象一整个指到其他地方去了，原来的响应并没有失去=>你搬家了，房子不是你的了，它不管怎么装修，与你无关
        shoppingCart = sourceCart
        console.log(shoppingCart);
    }

    // // ref包裹的对象
    // let addCount = (index)=>{
    //     shoppingCart.value[index].count++
    // }
    // let alignCart = (index)=>{
    //     // 对象一直没变，只是内含的东西变了=>你长大了，你还是你
    //     shoppingCart.value = sourceCart
    //     console.log(shoppingCart);
    // }
</script>
```

> *reactive修改整个对象其实可以利用浅拷贝assign做出类似的效果*
>
> *但是底层其实只是将内部属性一个一个重新赋值*

#### 区别与使用场景

> ==**总结就一句话，一般除了嵌套很深的都用ref**==；ref唯一烦人的地方就是需要给对象加一个value再赋值，不过这个也可以通过插件解决，笔者认为不如手动挡，在这里就不介绍了



下面是官方文档的描述，有兴趣的可以看看，重点是最后一句哈哈。



##### `reactive()` 的局限性[](https://cn.vuejs.org/guide/essentials/reactivity-fundamentals.html#limitations-of-reactive)

`reactive()` API 有一些局限性：

1. **有限的值类型**：它只能用于对象类型 (对象、数组和如 `Map`、`Set` 这样的[集合类型](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects#keyed_collections))。它不能持有如 `string`、`number` 或 `boolean` 这样的[原始类型](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)。

2. **不能替换整个对象**：由于 Vue 的响应式跟踪是通过属性访问实现的，因此我们必须始终保持对响应式对象的相同引用。这意味着我们不能轻易地“替换”响应式对象，因为这样的话与第一个引用的响应性连接将丢失：

   js

   ```
   let state = reactive({ count: 0 })
   
   // 上面的 ({ count: 0 }) 引用将不再被追踪
   // (响应性连接已丢失！)
   state = reactive({ count: 1 })
   ```

3. **对解构操作不友好**：当我们将响应式对象的原始类型属性解构为本地变量时，或者将该属性传递给函数时，我们将丢失响应性连接：

   js

   ```
   const state = reactive({ count: 0 })
   
   // 当解构时，count 已经与 state.count 断开连接
   let { count } = state
   // 不会影响原始的 state
   count++
   
   // 该函数接收到的是一个普通的数字
   // 并且无法追踪 state.count 的变化
   // 我们必须传入整个对象以保持响应性
   callSomeFunction(state.count)
   ```

==**由于这些限制，我们建议使用 `ref()` 作为声明响应式状态的主要 API。**

#### `toref`和`torefs`

> 解构对象时采用`toref`结构出对象中的一个元素，`torefs`解构出整个对象，重要的是它们解构出来还具有原来的响应式特点

## 计算属性computed

> 案例代码
>
> ```vue
> <template>
>     <div class="Intro">
>         <h3>组合姓名</h3>
>         <p>姓：<input type="text" v-model="_name1"></p>
>         <p>名：<input type="text" v-model="_name2"></p>
>         <p>姓名：<span>{{ fullName }}</span></p>
>         <button @click="changeName">恢复为默认</button>
>     </div>
> </template>
> <!-- 注意这里的name就是组件名 -->
> <script setup name="Introduce">
>     import {ref,computed} from "vue";
> 
>     let _name = ref("");
>     let _name1 = ref("");
>     let _name2 = ref("");
> 
>     // // 普通版
>     // let fullName = computed(() => {
>     //     return _name1.value.slice(0,1).toUpperCase()+_name1.value.slice(1) + _name2.value;
>     // })
>     // 全能版
>     let fullName = computed({
>         get() {
>             return _name1.value.slice(0, 1).toUpperCase() + _name1.value.slice(1) + " " + _name2.value;
>         },
>         set(val) {
>             let nameArr = val.split(" ");
>             _name1.value = nameArr[0];
>             _name2.value = nameArr[1];
>             _name.value = val;
>         }
>     })
>     let changeName = () => {
>         fullName.value = "佚 名";
>     }
> </script>
> <style scoped>
>     .Intro {
>         width: 300px;
>         /* height: 300px; */
>         background-color: #f99;
>         border-radius: 10px;
>         box-shadow: 3px 5px 2px 2px rgba(0, 0, 0, .6);
>         flex-direction: column;
>         text-align: center;
>     }
>     button {
>         display: block;
>         margin: 10px auto;
>         cursor: pointer;
>         border: 1px solid #fff;
>         background-color: #09f;
>         color: #fff;
>         width: 100px;
>         height: 30px;
>         border-radius: 5px;
>     }
>     .alignBtn {
>         height: 50px;
>     }
> </style>
> ```

### 中规中矩的computed

默认getter

```vue
// 普通版
let fullName = computed(() => {
    return _name1.value.slice(0,1).toUpperCase()+_name1.value.slice(1) + _name2.value;
})
```



### 致敬java的computed

getter&setter

```vue
// 全能版
let fullName = computed({
    get() {
        return _name1.value.slice(0, 1).toUpperCase() + _name1.value.slice(1) + " " + _name2.value;
    },
    set(val) {
        let nameArr = val.split(" ");
        _name1.value = nameArr[0];
        _name2.value = nameArr[1];
        _name.value = val;
    }
})
```



### 与function的区别

> computed代码块只执行一次，function调用一次执行一次



## watch监视

> vue3使用功能第一步，**导入**
>
> import {watch,ref,reactive} from 'vue'

### 基本类型的监视

`stopWatch = watch(var,(newval,oldval)=>{...if()stopWatch()})`

### 对象类型的监视

#### ref

> 参数列表 对象 回调函数 配置项

`stopWatch = watch(obj,(newval,oldval)=>{...if()stopWatch()},{deep:true,immediate:true})`

#### reactive

> 默认开启深度监视，即内部属性变化就会触发

`stopWatch = watch(obj,(newval,oldval)=>{...if()stopWatch()})`

### 对象中属性的监视

> 官方文档要求第一个参数要么时基本类型，要么是对象类型，要么是返回一个值的函数
>
> 所以对象中的属性采用第三种方法
>
> **为了方便，将函数直接写成箭头函数**
>
> **<u>考虑到对象中的属性可能仍然为对象，所以加上配置项，打开深度监视</u>**

`stopWatch = watch(()=>obj.var,(newval,oldval)=>{},{deep:true})`

### watchEffect“全局自动监视”

> 导入
>
> import {ref,watchEffect} from 'vue'

`watchEffect(()=>{pass})`

> **写法**相当于watch监听省去了参数列表中的第一个参数
>
> 应该是相对watch比较耗流

## 标签的ref属性

> 标签的一种标记，方便不使用原生JS选定元素

### 普通html标签上

### 组件标签上

> ①有保护机制，直接拿只能拿到一个保护对象
>
> ②在组件中导入defineExpose并且在脚本（script）最后声明可以被查看的属性defineExpose({var1,var2...})
>
> ③这样才能拿到组件的一些有用信息
