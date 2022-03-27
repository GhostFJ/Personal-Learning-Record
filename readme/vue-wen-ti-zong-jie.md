---
description: vue开发遇到的一些问题小记
---

# Vue问题总结

#### 1、问题描述： <a href="#pvl0g" id="pvl0g"></a>

在使用vue-pdf插件加载本地PDF文件进行预览时，报如下错误：\
'Warning: Ignoring invalid character "33" in hex string'\
'Warning: Ignoring invalid character "79" in hex string'...\
**解决方法：**\
在vue-cli3脚手架搭建的项目中，读取本地的PDF文件需要放到public文件夹中，在引用时，不能使用相对路径，且‘/’即表示public文件夹(需略去public)

\


#### 2、问题描述： <a href="#gxieo" id="gxieo"></a>

VUE项目中eslint报错： Expected linebreaks to be 'LF' but found 'CRLF'\
**解决方法：**右下角或者修改默认配置\
Windows系统下文本文件的换行符是： 回车+换行CR/LF即 \r\n或^M\
linux/unix系统下文本文件的换行符是：换行LF即 \
Mac OS系统下文本文件的换行符：回车CR即 \r或^M

\


#### 3、问题描述 <a href="#lytlw" id="lytlw"></a>

使用常量替代 Mutation 事件类型\
使用常量替代 mutation 事件类型在各种 Flux 实现中是很常见的模式。这样可以使 linter 之类的工具发挥作用，同时把这些常量放在单独的文件中可以让你的代码合作者对整个 app 包含的 mutation 一目了然

```
// mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'

// store.js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.Store({
state: { ... },
mutations: {
// 我们可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
[SOME_MUTATION] (state) {
// mutate state
}
}
})

同理，对于Action，会经常用到 ES2015 的 参数解构 (opens new window)来简化代码
（特别是我们需要调用commit很多次的时候）

actions: {
increment ({ commit }) {
commit('increment')
}
}
```

\


#### 4、MQTT使用： <a href="#luvdm" id="luvdm"></a>

\


```
通过协议指定使用的连接方式
// ws 未加密 WebSocket 连接
// wss 加密 WebSocket 连接
// mqtt 未加密 TCP 连接
// mqtts 加密 TCP 连接
// wxs 微信小程序连接
// alis 支付宝小程序连接
```

\


#### 5.关于Vue中 style的scoped用法： <a href="#zrb6h" id="zrb6h"></a>

\
发现使用的组件设置的样式在页面中不起作用，被层叠覆盖\
当一个style标签拥有scoped属性时候，它的css样式只能用于当前的Vue组件，可以使组件的样式不相互污染。如果一个项目的所有style标签都加上了scoped属性，相当于实现了样式的模块化。\
scoped看起来很好用，当时在Vue项目中，当我们引入第三方组件库时(如使用vue-awesome-swiper实现移动端轮播)，需要在局部组件中修改第三方组件库的样式，而又不想去除scoped属性造成组件之间的样式覆盖。这时我们可以通过特殊的方式穿透scoped。如 v-deep

\


#### 6，this.$v是使用vuelidate后注册到vue中的，可以打印出来看看，注意使用 <a href="#bi66n" id="bi66n"></a>

\


#### 7、Unexpected token u in JSON at position 0： <a href="#whxfr" id="whxfr"></a>

我出现这个错误的原因是：我使用localStorage或者sessionStorage存入本地数据时，\
存入了一个值为 Undefined ,当我在去取出来转换为JSON对象的时候，就是出现 Unexpected token u in JSON at position 0 报错\
所以，当你存入数据和使用JSON.parse转换时需要判断一下，这个值不能为Undefined，才能进行JSON.parse

本质上的问题就是json转换的对象时个空的

\


#### 8.value.push is not a function <a href="#xsznb" id="xsznb"></a>

在使用el-select 和el-option时，注意， el-select绑定的值，如果是多选，一定要是个数组，同时注意在请求后台数据时，如果赋值成了字符串，就会出现这个错误

\


#### 9、put无反应： <a href="#clero" id="clero"></a>

在写一个blog系统的测试开发过程中，发现put修改一直没有结果，发现是后端没有获取到数据，因为后端是Query获取拼接到url后面的参数，而我们的请求头是 application/json;charset=UTF-8，以payload的形式，所有取不到，故修改后端代码

\


#### 10、渲染问题 <a href="#isxgq" id="isxgq"></a>

**Error in render: "TypeError:Cannot read property xxx of undefined"**\
模板在渲染时候，读取对象中的某个对象的属性值时，这个对象不存在\
**原因：**\
vue渲染机制中：\
异步数据先显示初始数据，再显示带数据的数据，所以上来加载时还是一个空对象，没有从后台取到数据\
解决：\
在前面加v-if判断条件，如果取不到则不加载即可，（注意，不能用v-show，v-show的机制是加载后，根据条件判断是否显示）

\


#### 11、vue中的transition <a href="#wbtbj" id="wbtbj"></a>

\
Vue 提供了 transition 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡

\


1. 条件渲染 (使用 v-if)
2. 条件展示 (使用 v-show)
3. 动态组件
4. 组件根节点

\


#### 12、vue的ref 问题 <a href="#pn8ye" id="pn8ye"></a>

写在标签上时：this.$refs.名字  获取的是标签对应的dom元素\
ref 写在组件上时：这时候获取到的是 子组件（比如counter）的引用

\


#### 13、scss中引用图片路径， <a href="#cnok5" id="cnok5"></a>

可以这么写\
background: url("\~@/assets/img/footer.jpg") no-repeat;

\


#### 14、关于vue的 nextTick <a href="#bgkww" id="bgkww"></a>

newxtTick是在DOM数据刷新之后进行的延时处理，一般可用于create等地方

\


#### 15、js中如果想用变量值来做对象的属性名， <a href="#ccvng" id="ccvng"></a>

用中括号 \[]：如下示例

```
for (const item of temp) {
  const index = item.pid
  if (index in data) {
  	data[index].push(item)
  } else {
    data[index] = []
    data[index].push(item)
  }
}
```

\


#### 16、axios默认不带cookie，开启需要在 配置中加：withCredentials: true  <a href="#lubws" id="lubws"></a>

#### 17、样式穿透示例、内容换行 <a href="#emq5n" id="emq5n"></a>

```
.el-table{
  ::v-deep .cell {
 		white-space: pre-line !important;/保留换行符/
  }
}
```

\


#### 18、elementUI日期选择时间器中default-time属性无效问题 <a href="#fkljf" id="fkljf"></a>

把value-format中的h换成H即可\
`value-format="yyyy-MM-dd HH:mm:ss"`

\


#### 19、一种当前页面模糊搜索的实现方式 <a href="#k1id5" id="k1id5"></a>

利用el-table的 :row-class-name 属性配合css的display，也可以实现模糊搜索，

#### 20、watch监听data里面的对象时即 {}元素， <a href="#ool49" id="ool49"></a>

1、可以使用深度监听deep，但是会浪费开销，

2、可以使用字符串或通过计算属性来间接调用 `'object.prop'() {}`\
Vue中watch对象内属性的方法 - SegmentFault 思否

\


#### 21、css 上下两个按钮对齐，只需要在两者之间插入一个换行即：\<br> <a href="#dw5fn" id="dw5fn"></a>

\


#### 22、slice() ,js的slice无参调用可以将对象转为数组(elmentui源码参考) <a href="#rpy1j" id="rpy1j"></a>

\


#### 23、v-model的本质： <a href="#bwknl" id="bwknl"></a>

\


1. **v-bind :  绑定响应式数据**
2. **触发 input 事件 并传递数据  (核心和重点)**\
   监听原生组件的事件, 当获取到原生组件的值后把值通过调用 $emit('input' ,data) 方法去触发 input事件\
   使用了v-model的组件会自动监听 input 事件,

\


* 并把这个input事件所携带的值 传递给v-model所绑定的属性,
* 这样组件内部的值就给到了父组件了\


\


#### 24、css解决element-ui 的按钮点击移开不恢复问题： <a href="#tcmyg" id="tcmyg"></a>

```
.el-button:focus:not(.el-button:hover) {
  background: #0ca49b;
  color: #fff;
}
```

\


#### 25、window.opener: <a href="#gfgyf" id="gfgyf"></a>

返回打开当前窗口的那个窗口的引用，例如：在window A中打开了window B，B.opener 返回 A.

\


#### 26、reduce去重的一个例子：  <a href="#efl8h" id="efl8h"></a>

```
let colors = [{
  id: 0,
  colorName: 'red'
  },
  {
  id: 1,
  colorName: "orange"
  },
  {
  id: 1,
  colorName: "yellow"
  },
];
// 去除id重复的
let obj = {};
colors = colors.reduce((cur, next) => {
obj[next.id] ? "" : obj[next.id] = true && cur.push(next);
return cur;
}, [])
```

\


#### 27、Vue项目部署到非根路径（nginx为例） <a href="#sryvj" id="sryvj"></a>

1. 修改vue.config.js中的publicPath ，`比如http/101.20.12.9/a/路径， punlicPath：/a/`
2. 修改router的路径，增加base URL `如：base: '/runmode/'`
3. nginx里面配置的try\_file属性：

```
location ^~/m {
    try_files $uri $uri/  /m/index.html;
}
```

apache配置

```
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>
```

#### 28、vue之this.$route.query和this.$route.params的使用与区别 <a href="#vft7s" id="vft7s"></a>

http://localhost:3000/edit/123

如果是这种的URL就需要使用this.$route.params.id

http://localhost:3000/?token=ajdfyuafduyagddsa

如果是这种的URL就需要使用this.$route.query\


#### 29、生命周期的一些理解 <a href="#pxz8f" id="pxz8f"></a>

在created阶段，对浏览器来说，渲染整个HTML文档时,dom节点、css规则树与js文件被解析后，但是没有进入被浏览器render过程，上述资源是尚未挂载在页面上，也就是在vue生命周期中对应的created

阶段，实例已经被初始化，但是还没有挂载至$el上，所以我们无法获取到对应的节点，但是此时我们是可以获取到vue中data与methods中的数据的

\


在beforeMount阶段，实际上与created阶段类似，节点尚未挂载，但是依旧可以获取到data与methods中的数据。

\


在mounted阶段，对浏览器来说，已经完成了dom与css规则树的render，并完成对render tree进行了布局，而浏览器收到这一指令，调用渲染器的paint（）在屏幕上显示，而对于vue来说，在mounted阶段，vue的template成功挂载在$el中，此时一个完整的页面已经能够显示在浏览器中，所以在这个阶段，即可以调用节点了（关于这一点，在笔者测试中，在mounted方法中打断点然后run，依旧能够在浏览器中看到整体的页面）。

\


#### 30、POST、GET、RequestParam、ReqestBody、FormData、request payLoad简单认知 <a href="#ili6x" id="ili6x"></a>

\


post请求体是通过请求头中的Content-Type来区分的：formData形式的Content-Type为application/x-www-form-urlencoded ；而request payload形式的请求体Content-Type为application/json或multipart/form-data 。

使用@RequestBody注解接收request payload形式的请求体参数；使用@RequestParam注解接收formData形式的请求体参数。

\


很多时候，我们可能需要同时调用多个后台接口，就会高并发的问题，一般解决这个问题方法：

axios.all 和 axios.spread

#### 31、Vue与CAS <a href="#yhxxv" id="yhxxv"></a>

参考一：[基于CAS实现SSO单点登录](https://zhuanlan.zhihu.com/p/25007591)

参考二：[一篇文章彻底弄懂CAS实现SSO单点登录原理](https://www.cnblogs.com/wangsongbai/p/10299655.html)

参考三：[基于Vue-Element-Admin前端接入SSO](https://blog.csdn.net/kielin/article/details/107714953?depth\_1-utm\_source=distribute.pc\_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-18.control)

**基础模式 SSO 访问流程主要有以下步骤：**

1. 访问服务： SSO 客户端发送请求访问应用系统提供的服务资源。
2. 定向认证： SSO 客户端会重定向用户请求到 SSO 服务器。
3. 用户认证：用户身份认证。
4. 发放票据： SSO 服务器会产生一个随机的 Service Ticket 。
5. 验证票据： SSO 服务器验证票据 Service Ticket 的合法性，验证通过后，允许客户端访问服务。
6. 传输用户信息： SSO 服务器验证票据通过后，传输用户认证结果信息给客户端。

\


**基本的协议过程如下图：**

![](https://cdn.nlark.com/yuque/0/2021/png/22252400/1631884459175-423e1cb7-582f-417e-b218-52b87adfee88.png)

[\
](https://blog.csdn.net/kielin/article/details/107714953?depth\_1-utm\_source=distribute.pc\_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-18.control)

#### 32、this.$confirm的异步问题 <a href="#akq3h" id="akq3h"></a>

由于this.$confim 采用的promise方式，不能阻断下面代码的执行，所以可以采用async/await等待返回结果

即采用：`await this.$confirm(...)`

#### 33、vite+vue3报错，require is not defined <a href="#dx23p" id="dx23p"></a>

目前好像没有办法解决，只能使用import导入替换

#### 34、css实现上标效果 <a href="#v7tnj" id="v7tnj"></a>

sup标签 配合

```
sup {
  vertical-align: 75%;
  line-height: 5px;
  font-size:11px;
}
```

#### 35、使用html的form表单注意 <a href="#gekph" id="gekph"></a>

form表单中，button的type属性默认是submit，当type的值是submit时，点击button就会自动刷新

手动设置type字段的值就解决了，即type=“button”

#### 36、关于复杂判断（if/else、switch）的优雅写法 <a href="#gtdyy" id="gtdyy"></a>

[参考博客](https://blog.csdn.net/qq\_40259641/article/details/83866457)

使用Map或者对象，

将判断条件作为对象的属性名，

将处理逻辑作为对象的属性值

一一关联起来，使用时按key-value取出即可，因为方法也是可以做键值的，所以很方便

甚至可以在Map中，以Object作为key

在有重复逻辑时，以正则作为key，更方便

1. 一元判断时：存到Object里
2. 一元判断时：存到Map里
3. 多元判断时：将condition拼接成字符串存到Object里
4. 多元判断时：将condition拼接成字符串存到Map里
5. 多元判断时：将condition存为Object存到Map里
6. 多元判断时：将condition写作正则存到Map里

```
例一
const actions = new Map([
  [1, ['processing','IndexPage']],
  [2, ['fail','FailPage']],
  [3, ['fail','FailPage']],
  [4, ['success','SuccessPage']],
  [5, ['cancel','CancelPage']],
  ['default', ['other','Index']]
])
/**
 * 按钮点击事件
 * @param {number} status 活动状态：1 开团进行中 2 开团失败 3 商品售罄 4 开团成功 5 系统取消
 */
const onButtonClick = (status)=>{
  let action = actions.get(status) || actions.get('default')
  sendLog(action[0])
  jumpTo(action[1])
}


例二

const actions = new Map([
  ['guest_1', ()=>{/*do sth*/}],
  ['guest_2', ()=>{/*do sth*/}],
  ['master_1', ()=>{/*do sth*/}],
  ['master_2', ()=>{/*do sth*/}],
  ['default', ()=>{/*do sth*/}],
])

/**javascript
 * 按钮点击事件
 * @param {string} identity 身份标识：guest客态 master主态
 * @param {number} status 活动状态：1 开团进行中 2 开团失败 3 开团成功 4 商品售罄 5 有库存未开团
 */
const onButtonClick = (identity,status)=>{
  let action = actions.get(`${identity}_${status}`) || actions.get('default')
  action.call(this)
}

```

#### 37、Vuex关于action和mutation使用 <a href="#jgrk5" id="jgrk5"></a>

首先：

mutations在请求数据的时候是同步的; 而actions是异步的

所以当有异步请求或异步操作时，使用action，并推荐使用mutation去修改state

#### 38、Vue权限控制 <a href="#ph5bf" id="ph5bf"></a>

1、前端固定多套路由

2、前端根据用户配置动态生成路由表

3、后端生成返回给前端

按钮级别的粒度：可以vuex 配合自定义指令实现，或者v-if，自定义指令inserted时判断权限，如果没有，移除此子节点

#### 39、vue的深层对象更新与$forceUpdate <a href="#gjgoj" id="gjgoj"></a>

对于复杂对象的修改，要按照规范去修改，例如

```
change: function(index) {//增加性别属性
    this.list[index].sex = '男';
},
clear: function() {//清空数组
    this.list.length = 0;
}
```

vue建议对于深层修改，最好使用`$set`方法，同时vue也不建议直接修改length，可以给一个空数组来置空。如下

```
change: function(index) {//增加性别属性
    this.$set(this.list[index],'sex','男')
},
clear: function() {//清空数组
    this.list=[];
}
```

而使用`$forceUpdate`就相当于按照最新数据给渲染一下，强制更新，它仅仅影响实例本身和插入插槽内容的子组件，而不是所有子组件

#### 40、Vue中使用$emit传递多个参数时 <a href="#hb8ug" id="hb8ug"></a>

接受的父组件处理函数参数要设置为：`arguments`，存在`arguments`数组中

#### 41、v-if无效不及时响应的问题 <a href="#saq1w" id="saq1w"></a>

参考vue的响应式原理，由于JS的限制，Vue 不能检测数组和对象的变化

**对于对象：**

Vue 无法检测 property 的添加或移除

可以使用 `Vue.set(object, propertyName, value)` 方法向嵌套对象添加响应式 property。

还可以使用 vm.$set 实例方法，这也是全局 Vue.set 方法的别名

**对于数组：**

1. 当你利用索引直接设置一个数组项时，例如：vm.items\[indexOfItem] = newValue
2. 当你修改数组的长度时，例如：vm.items.length = newLength

\


为了解决第一类问题，以下两种方式都可以实现和 vm.items\[indexOfItem] = newValue 相同的效果，同时也将在响应式系统内触发状态更新：

\


```
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)
// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)

// 也可以使用 vm.$set 实例方法，该方法是全局方法 Vue.set 的一个别名：

vm.$set(vm.items, indexOfItem, newValue)
```

\


为了解决第二类问题，你可以使用 splice：`vm.items.splice(newLength)`

\


#### 42、VUE中iframe结合window.postMessage实现跨域通信 <a href="#pj8xt" id="pj8xt"></a>

首先两个知识点：

**1、`window.postMessage`**

**语法：**

`otherWindow.postMessage(message, targetOrigin, [transfer]);`

**otherWindow**

其他窗口的一个引用，比如iframe的contentWindow属性、执行window.open返回的窗口对象、或者是命名过或数值索引的window.frames。

**message**

将要发送到其他 window的数据。它将会被结构化克隆算法序列化。这意味着你可以不受什么限制的将数据对象安全的传送给目标窗口而无需自己序列化。\[1]

**targetOrigin**

通过窗口的origin属性来指定哪些窗口能接收到消息事件，其值可以是字符串"\*"（表示无限制）或者一个URI。在发送消息的时候，如果目标窗口的协议、主机地址或端口这三者的任意一项不匹配targetOrigin提供的值，那么消息就不会被发送；只有三者完全匹配，消息才会被发送。这个机制用来控制消息可以发送到哪些窗口；例如，当用postMessage传送密码时，这个参数就显得尤为重要，必须保证它的值与这条包含密码的信息的预期接受者的origin属性完全一致，来防止密码被恶意的第三方截获。如果你明确的知道消息应该发送到哪个窗口，那么请始终提供一个有确切值的targetOrigin，而不是\*。不提供确切的目标将导致数据泄露到任何对数据感兴趣的恶意站点。

transfer 可选

是一串和message 同时传递的 Transferable 对象. 这些对象的所有权将被转移给消息的接收方，而发送一方将不再保有所有权。

**window.postMessage()** 方法可以安全地实现跨源通信

从广义上讲，一个窗口可以获得对另一个窗口的引用（比如 `targetWindow = window.opener`），然后在窗口上调用 `targetWindow.postMessage()` 方法分发一个 `MessageEvent` 消息。接收消息的窗口可以根据需要自由处理此事件 。传递给 `window.postMessage()` 的参数（比如 message ）将通过消息事件对象暴露给接收消息的窗口。

即：

通过**postMessage**不同窗口进行通信，通过

**`window.addEventListener('message', this.listener)`**

**监听事件**

**2、\<iframe>**

HTML内联框架元素 (\<iframe>) 表示嵌套的browsing context。它能够将另一个HTML页面嵌入到当前页面中。

[iframe-MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe)

**通过这两个组合，可以实现嵌入页面和跨域通信**

**3、示例**

```
// 父页面
// 父页面监听子页面传来的信息
mounted() {
    window.addEventListener('message', this.handleMessage);
    this.iframeWin = this.$refs.iframe.contentWindow;
},

methods: {
  handleMessage (event) {
    const data = event.data.data
    if(data.info === "success"){
      alert(data.data)
    }
  }
}



// 子页面，window.parent获得父页面
sendMessage() {
    // 向父页面发送信息
    window.parent.postMessage({
        data: {
            info:"success",
            data:"我是子页面的test！"
        }
    }, '*');
}


// 父页面向子页面传值同理
sendMessage () {
    // 向子页面传数据,需要注意这里没有parent
    this.iframeWin.postMessage({
        info: 'success',
        data: "我是来自父页面的data!"
    }, '*')
}
```

\


#### 43、记ESLint的一个报错 <a href="#eppkm" id="eppkm"></a>

`ESLint: Cannot read property 'loc' of undefined...`

解决方法：

Eslint的rules下

```
-       'indent': [  // 删除
+       '@typescript-eslint/indent': [
            'error',
            2
        ],
```

看样子是由于缩进引起的？

#### 44、elementui中el-table的row-style不起作用的原因 <a href="#uemxt" id="uemxt"></a>

应该返回一个对象，Object，而不是字符串
