# 知识笔记总结

## [js基础知识](./js-note/js基础知识.md)
## [Math方法](./js-note/Math方法.md)
## [标识符,作用域，预解析，闭包](./js-note/标识符,作用域，预解析，闭包.md)
## [定时器](./js-note/定时器.md)
## [数组的方法](./js-note/数组的方法.md)
## [字符串方法](./js-note/字符串方法.md)
## [面向对象](./js-note/面向对象.md)
## [日期对象](./js-note/日期对象.md)
## [正则表达式](./js-note/正则表达式.md)
## [BOM](./js-note/BOM.md)
## [DOM](./js-note/DOM.md)
## [ES6](./js-note/ES6.md)
## [git命令](./js-note/git命令.md)
## [jQuery](./js-note/jQuery.md)
## [js兼容性](./js-note/js兼容性.md)
## [Node.js](./js-note/Node.js.md)
## [promise](./js-note/promise.md)
## [React](./js-note/React.md)
## [vue 路由](./js-note/vue 路由.md)
## [本地服务建立](./js-note/本地服务建立.md)
## [饿了么element组件](./js-note/饿了么element组件.md)
---
## data
> 每个 Vue 实例都会代理其 data 对象里所有的属性

```js
let a = 1;
let b = { num:100 }

let vm = new Vue({
    el:'#box',
    data:{
        n:a,
        m:b
    }
})

vm.n = 9;
console.log(vm.n)//9
console.log(a)//1

a = 5;
console.log(vm.n)//9
console.log(a)//5
//如果是简单类型的值，实例化后再改变其值，并不会改变data中应用该值时的值。

console.log(vm.m.num)//100
vm.m.num = 999;
//b.num = 999;
console.log(vm.m.num)//999
console.log(b.num)//999
//不管先改data中的数据还是先改data引用的数据，都会影响整体数据，发生相同的结果。
	</script>
```

## v-if和v-show
```js
<div id="box">
    <div v-show="tab">v-show</div>
    <div v-if="tab">v-if</div>
    <input type="button" @click="tab = !tab" />
</div>

new Vue({
    el:"#box",
    tab:true
})
```
> v-if:为true时：结构显示； 为false时：结构在HTML中消失，找不到了；
> v-show:为true时：display:block; 为false时：结构还在，只是display：none;了

## v-if
> v-if是条件渲染指令，它根据表达式的真假来删除和插入元素

> 语法：v-if="expression"

```js
<body>
    <div id="app">
        <h1>Hello, Vue.js!</h1>
        <h1 v-if="yes">Yes!</h1>
        <!--引用data中的yes属性，属性值为true，所以显示-->
        
        <h1 v-if="no">No!</h1>
        <!--引用的属性值为false，所以不渲染-->
        
        <h1 v-if="age >= 25">Age: {{ age }}</h1>
        <!--js表达式，返回true，所以渲染-->
        
        <h1 v-if="name.indexOf('jack') >= 0">Name: {{ name }}</h1>
        <!--js表达式返回false，所以不渲染-->
    </div>
</body>
    
var vm = new Vue({
    el: '#app',
    data: {
        yes: true,
        no: false,
        age: 28,
        name: 'keepfool'
    }
})
```

    为什么Vue实例可以直接访问定义在选项对象的data属性呢？这是因为每个Vue实例都会代理其选项对象里的data属性

> 如果v-if返回的值是false，则该标签不会进行渲染，结构中没有该结构

## v-show
> 如果上面的v-if的结构中，把v-if换成v-show，会出现相同的界面，但是实现原理却不相同，v-show是通过css属性中的display控制block和none来时实现的

## v-else
> 可以用v-else指令为v-if添加一个“else块”。v-else元素必须立即跟在v-if元素的后面——否则它不能被识别。v-else不需要写表达式，只需要写c-else即可

```js
<h2 v-if="status">if标题h2</h2>
<h2 v-else>else标题h2</h2>

new Vue({
el:'#bo',
data:{
	status:false
})

------------结果为-------------
//else标题h2
```
只有当v-if中的表达式返回的值为false时，才会执行v-else，

## v-bind
> 属性不能使用双大括号形式绑定，只能使用v-bind指令

### 预期值

> 如 v-bind:class="classProperty" 中，v-bind 是指令，: 后面的 class 是参数，而 classProperty 则在官方文档中被称为“预期值”

### 支持单一的js表达式(v-for 除外)
* 执行运算

```js
<div id="app">
    <p v-bind:title="t1 + ' ' + t2">html属性不能使用双大括号形式绑定，只能使用v-bind指令</p>
</div>

var vm = new Vue({
    el: '#app',
    data: {
        t1: 'title1',
        t2: 'title2'
    }
})

------结果为--------
<div id="app">
    <p title="title1 title2">html属性不能使用双大括号形式绑定，只能使用v-bind指令</p>
</div>
```
* 执行函数

```js
<div id="app">
    <p v-bind:title="getTitle()">html属性不能使用双大括号形式绑定，只能使用v-bind指令</p>
</div>

var vm = new Vue({
    el: '#app',
    data: {
        getTitle: function () {
            return 'title content';
        }
    }
})
---------结果为------------
<div id="app">
    <p title="title content">html属性不能使用双大括号形式绑定，只能使用v-bind指令</p>
</div>
```

### 支持的数据类型
* 对象类型

```js
<div id="app">
    <p v-bind:title="obj">content</p>
</div>

var vm = new Vue({
    el: '#app',
    data: {
        obj: {}
    }
}
-------结果为-----------
<div id="app">
    <p title="[object Object]">content</p>
</div>
```
属性值是对象obj的toString方法的返回值。
如果改变了对象obj的toString方法的返回值，响应的title属性值就会发生变化。如：
```js
let ob = {};

ob.toString = function(){
    return 'abcd'
}

var vm = new Vue({
    el: '#app',
    data: {
        obj: ob
    }
}
-------结果为-----------
<div id="app">
    <p title="abcd">content</p>
</div>
```
* 数组类型

```js
<div id="app">
    <p v-bind:title="arr">content</p>
</div>

var vm = new Vue({
    el: '#app',
    data: {
        arr: [1, 2, 3]
    }
})
--------=--结果为-----------
<div id="app">
    <p title="1,2,3">content</p>
</div>
```
调用的是数组的tostring方法。将每一项用逗号连接。

* number类型

```js
<div id="app">
    <p v-bind:title="num">content</p>
</div>

var vm = new Vue({
    el: '#app',
    data: {
        num:999
    }
})
--------结果为-----------
<div id="app">
    <p title="999">content</p>
</div>
```
把数字转为字符串类型后进行调用，数字0也会转为字符串0

* 布尔值类型

```js
<div id="app">
    <p v-bind:title="boo">content</p>
</div>

var vm = new Vue({
    el: '#app',
    data: {
        boo:false
    }
})
--------结果为-----------
<div id="app">
    <p>content</p>
</div>
```
如果是布尔类型，值如果为false，则不会渲染出队形的title="false",但是如果为true。则会出现对应的属性值
```js
<div id="app">
    <p v-bind:title="boo">content</p>
</div>

var vm = new Vue({
    el: '#app',
    data: {
        boo:true
    }
})
--------结果为-----------
<div id="app">
    <p title="true">content</p>
</div>
```
## vue对class和style的增强

### 基于对象对class的增强
```js
<div class="box">
	<p :class="tit" v-for="item,index in list">{{item}}{{'这是'+index}}</p>
</div>

let obj = {
	cla:true,
	cla2:false,
	cla3:'true',
	cla4:'false'
}
obj.toString = function(){
	let str = ''
	for(var attr in obj){
		if(obj[attr]){
			str+=obj[attr]+' ';//转为字符串进行拼接，后面加空格
		}
	}
	return str;
};
// 防止 toString 方法自身被遍历出来
Object.defineProperty(obj,'toString',{'enumerable':false})
new Vue({
	el:'.box',
	data:{
		list:[
			1,2,3,4,5
		],
		tit:obj//如果是对象，他会调用对象的toString方法，把它转为字符串类型
	}
})
-----------结果为------------
<p class="cla cla3 cla4">1这是0</p>
...
```
### 基于数组对class的增强
```js
<div id="app">
    <p v-bind:class="arr">content</p>
</div>

var arr = ['c1', 'c2', 'c3'];

arr.toString = function () {
    return this.join(' ');//调用数组的join方法，通过空格连接
};

var vm = new Vue({
    el: '#app',
    data: {
        arr: arr
    }
});
--------结果为----------
<div id="app">
    <p class="c1 c2 c3">content</p>
</div>
```

### 基于对象对style的增强
```js
<div class="box">
	<p :style="tit" v-for="item,index in list">{{item}}{{'这是'+index}}</p>
</div>

let obj = {
	background:'orange',
	color:'#fff'
}
obj.toString = function(){
	let str = ''
	for(var attr in obj){
		if(obj[attr]){//判断是否有属性值
			str+=attr +':'+ obj[attr] +';'
		}
	}
	return str;
};
new Vue({
	el:'.box',
	data:{
		list:[
			1,2,3,4,5
		],
		tit:obj
	}
})
---------结果为----------
<p style="background: orange; color: rgb(255, 255, 255);">1这是0</p>
```
### 基于数组对style的增强
```js
<div id="app">
    <p v-bind:style="arr">content</p>
</div>

<script>
var arr = [{
        color: "red"
    }, {
        backgroundColor: '#ddd'
    }, {
        fontSize: '20px'
    }];

arr.toString = function () {
    var str = '';
    arr.forEach(function (val, key) {
        for(var i in val) {
            str += i + ':' + val[i] + ';';
        }
    });
};

var vm = new Vue({
    el: '#app',
    data: {
        arr: arr
    }
})
------结果为------------
<div id="app">
    <p style="color: red; background-color: rgb(221, 221, 221); font-size: 20px;">content</p>
</div>
```

## 触发事件
```js
<div id="box">
    <div v-show="tab"></div>
    <div v-if="tab"></div>
    <input type="button" @keydown.13="fn" />
    <input type="button" @keydown.13="fn(1)" />
    <input type="button" @keydown.13="fn(1，$event)" />//$event表示事件对象
</div>

new Vue({
    el:"#box",
    tab:true,
    methods:{
        fn(n,ev){
            console.log(n);
            console.log(ev);
        }
    }
})
```
> vue中事件出发的时候，可以选择性的传入参数。.13代表回车enter。
以上代码第一个button点击的时候，执行的函数是未传入参数的fn，console打印的n是事件对象;
第二个button点击的时候，虽然带了小括号，但是并不会直接执行，打印的n是1；
第三个button点击的时候，n是第一个参数，所以是1，ev是事件对象；

## $refs
行间写 ref = 'abc'

## 判断是否添加类名

## 全局组件component
### 定义组件：Vue.component

    Vue.component('组件名',{选项对象})

```js
Vue.component('abc',{
    template:'<h3>这是标题</h3>'
})
```
**template模板中只能有一个根元素，就是只能多个元素被一个元素包裹**
> 第一个参数是组件名，
        1. 直接写单词，单词不能是HTML已有的HTML标签。
>       2. 可以使用驼峰命名法，但是在el规定区域内使用的时候，必须使用烤串命名法，
            如：component组件名为itemName,el区域内使用的时候不能写itemName,只能写item-name.
        3. 直接使用烤串命名法，
        如：如：component组件名为item-name,el区域内使用的时候直接写item-name.

> 第二个参数是这个组件的选项对象，用来配置这个组件的template，定义组件的模板。

### 组件的data --- 内部数据

```js
Vue.component('abc',{
    template:`
        <div>
            <input type="button" v-model="n" @click = "n++" />
        </div>
    `,
    
    data:function(){
        return {
            n:0
        }
    }
})
```
> 组件的data数据存放在组件的选项对象中，且data必须是一个函数，函数必须return出一个对象，否则警告。
 
### 组件的props --- 内部数据

```js
<div id="box">
    <abc title="123"></abc>
</div>

Vue.component('abc',{
    template:`
        <div>
            <input type="button" v-model="n" @click = "n++" />
            
        </div>
    `,
    
    props:['title'],
    
    data:function(){
        return {
            n:0
        }
    }
})

new Vue({
    el:'#box'
})
```

## 事件修饰符
* .stop  阻止单击事件冒泡

* .prevent 
* .capture  添加事件侦听器时使用事件捕获模式
* .self 只当事件在该元素本身（而不是子元素）触发时触发回调
* .once

## 按键修饰符
* .enter
* .tab 
* .delete (捕获 “删除” 和 “退格” 键)
* .esc
* .space
* .up
* .down
* .left
* .right
* .ctrl
* .alt
* .shift
* .meta --- win键
* .left
* .right
* .middle

## 局部组件components


## 生命周期

### 分三个阶段
1. 创建阶段


* 创建阶段：**beforeCreate(){}** 把data中的数据转为getter和setter，监听数据变化，此时console.log(vm.数据)是undefined

* 创建完成：**created（）{}** 把data中的数据绑定到实例上。此时console是可以打出来的

* 有无el属性：el的定义方法有两种，在实例中直接定义el,或者在实例外定义**vm.$mount('#box')**

* 有无template模板：有的话优先使用属性template中的模板，没有的话就拿el中的内容为模板

* 模板数据替换前：**beforeMounted(){}** 将数据和模板结合后替换结构前，此时内容还没有渲染出来

* 模板数据替换后：**mounted(){}** 此时数据结构模板全部渲染完成，这个时候可以获取dom,此时可以发送Ajax请求

* 数据更新前：**beforeUpdata(){}** 数据改变会触发更新前和更新后的函数

* 数据更新后：**updata(){}** 数据更新后触发

* 销毁实例：**vm.$destory()** 清除它与其他实例的链接，所有的事件监听器会被移除，所有的子实例也会被销毁。

* 销毁之前：**beforeDestroy(){}** 用v-if控制一个组件的显隐，会触发。
用处：通过v-if可以把一个元素销毁，但是它身上绑定的事件还在，长此以往会耗内存，所以在销毁前要把事件设为null。

* 销毁之后：**destoryed(){}** 销毁之后触发的函数

