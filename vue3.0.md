## Vue3.0

#### 1.安装vite打包工具和创建项目（推荐使用）

![image-20210607094354689](vue3.0.assets\image-20210607094354689.png)

#### 2.让ts识别.vue和mockjs

创建shim.d.ts，加入以下代码

![image-20210607094442504](vue3.0.assets\image-20210607094442504.png)

下边添加**declare module 'mockjs'**

#### 3.vue3.0安装

1.如果之前有2.0的需要先删除再安装最新的

 npm uni -g vue-cli

2.安装最新的脚手架

npm i -g @vue/cli

3.检查版本号

vue -V

4.创建项目(推荐使用vite创建方式)

vue  create 项目名称



#### 4.setup()初始值和方法

**setup由生命周期beforecreate和created转化而来**

重点：这里放弃了2.0的data()和methods，改而用**setup(){}** 里边可以**初始值和方法**,但是**<font color="red">最后必须return{}出去</font>**

##### 4.1初始值

使用vue中的**ref(初始值)**来初始值,  **import { ref } from 'vue**',**定义的值记得return出去**

##### 4.2定义方法

可以直接在setup中定义方法用来操作数据，一般以 **const 方法名 = ()=>{}形式**定义 ，方便外边使用，**定义的方法记得return出去**

![image-20210607114022827](vue3.0.assets\image-20210607114022827.png)

#### 5.计算属性computed

 **import { computed } from 'vue'**

computed计算属性也是在setup中定义，以**const 名称 = computed(()=>{ return })**形式使用,**最后也记得在最后的return中返回**

![image-20210607114528018](vue3.0.assets\image-20210607114528018.png)

#### 6.reactive/toRefs/toRef/ref

**常用reactive、toRefs、toRef(转化props中的数据时使用)**

**import { reactive,toRefs } from 'vue'**

##### 6.1reactive

作用：和ref()方法类似都是用来添加响应式数据的(只能存放对象类型)，通过**reactive()中可以存放对象**

用法：一般以**const  state=reactive({数据值})**形式定义，以**state.数据**使用

**最后把state通过return返回出去, 这样外边也是通过state.数据使用**

##### 6.2toRefs/toRef/ref

作用：一般和**reactive配合使用**，让数据**在外边的使用**和vue2.0中一样

用法：最后return出去的时候通过**...toRefs(state)** 将响应式对象state返回出去

![image-20210607120306699](vue3.0.assets\image-20210607120306699.png)

补充：toRef：`toRef` 是将某个对象中的**某个值转化为响应式数据**，其接收两个参数，第一个参数为 `obj` 对象；第二个参数为对象中的属性名 ,和ref作用相同，但是两者有区别

区别：

- ref: **拷贝**了一份原数据，修改响应式数据不会影响原数据，ui会**自动更新**,**可以用来获取dom元素**

- toRef：**引用**原数据，修改响应式数据会影响原数据，ui**不会更新**，如**props中的数据**

  ![image-20210610141335313](vue3.0.assets\image-20210610141335313.png)

![image-20210608105108496](D:\zhangjiahui53\Desktop\笔记\vue3.0.assets\image-20210608105108496.png)

#### 7.setup()的参数props和context

作用：**没有this去实现父传子，子传父**

##### 7.1props(父传子)

**第一个参数props**可以将props中父组件传过来的数据**以props参数传给初始值**;

![image-20210607124710245](vue3.0.assets\image-20210607124710245.png)

##### 7.2context(子传父)

使用**context.emit()实现子传父**

![image-20210607124922062](vue3.0.assets\image-20210607124922062.png)

#### 8.侦听器watch

 **import { watch} from 'vue'**

**watch(监听源，回调，{deep/immediate})**

##### 8.1不指定监听源

**watch(()=>{})**

![image-20210607125343854](vue3.0.assets\image-20210607125343854.png)

##### 8.2指定监听源

1.普通的**ref**数据，**watch(数据,()=>{})**

2.**响应式对象**中的数据，**watch( ()=>state.数据,(new,old,clean)=>{} )**

3.**多个数据源**使用**数组**指定,**watch([数据1，数据2],()=>{})**

clean用来清理异步事务，当监听器多次重复监听时触发

![image-20210607130253791](vue3.0.assets\image-20210607130253791.png)

补充：watchEffect

![image-20210611145941403](vue3.0.assets\image-20210611145941403.png)

#### 9.生命周期

**beforeCreate和created替换为setup，执行顺序不变;**

![image-20210607160347817](vue3.0.assets\image-20210607160347817.png)

#### 10.vuex/vue-router

1.使用vuex中的useStore ,**import** {useStore} **from** 'vuex'

2.const store = useStore()

3.通过store.数据  / store.dispatch等去使用

补充：**import** {useRoute} **from** 'vue-router'    **路由也是同理使用**

#### 11.defineComponent

**import** { defineComponent } **from** 'vue'

作用：用来包裹setup,可以区分setup的参数

![image-20210608094934595](vue3.0.assets\image-20210608094934595.png)

#### 12.getCurrentInstance(重)

总结：**<font color='red'>相当于this的作用,指向的是vue的实例，推荐使用instance.proxy</font>**

**import** { getCurrentInstance } **from** 'vue'

概述：一个很重要的方法，获取当前组件的实例、上下文来操作router和vuex等

例如： **instance.proxy.$router.push('路由名') /   instance.proxy.$toast() /    instance.proxy.$store** ,

**推荐：使用里边的proxy(响应式)  在开发环境和生产环境都能访问到组件的上下文**

![image-20210608095329815](vue3.0.assets\image-20210608095329815.png)

#### 13.获取Dom元素

1. 先给目标元素的 `ref` 属性设置一个值，假设为 `el`
2. 然后在 `setup` 函数中调用 `ref` 函数，值为 `null`，并赋值给变量 `el`，这里要注意，该变量名必须与我们给元素设置的 `ref` 属性名相同
3. 把对元素的引用变量 `el` 返回（return）出去

补充：设置的元素引用变量只有在**组件挂载后**才能访问到，因此在**挂载前对元素进行操作都是无效的**

当然如果我们引用的是一个组件元素，那么获得的将是该组件的实例对象

**可以通过.value属性获取到此dom/组件下定义的一些方法和属性**

![image-20210609084737356](vue3.0.assets\image-20210609084737356.png)

![image-20210609084726876](vue3.0.assets\image-20210609084726876.png)