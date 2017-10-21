# Node.js


---

## 一个文件就是一个模块

```js
function add(a,b){
    return a+b
}
```

> 暴露本模块的API，要把要暴露的函数挂载在module.exports上。
当使用require加载这个模块的时候，

## 加载一个模块

### 加载一个文件模块
* 语法：
    `require('./文件的相对或绝对路径') --- 路径必写`

### 加载一个内置模块
> 在安装node时就引入的模块，用原生js写的
* 语法：
    `require（'模块名'）`

### 文件夹模块（第三方模块）
> 文件夹下有个src文件夹，该文件夹下存引入的模块。直接require（'文件夹名'）是不可以的，因为没有入口文件，需要在文件夹下定义一个package.json,通过npm i引入一个package.json，然后在main定义模块的相对json的路径

### 模块的查找顺序
> 内置模块和文件夹模块共存时，会先找内置模块，如果内置模块和文件夹模块同名，则会找到内置模块，不会找文件夹模块。

## CMD 和 AMD
> 主要用在浏览器端

* cmd:seajs 依赖就近的原则
* amd:requireJs 依赖前置的原则

## CommonJS规范

> 每个文件就是一个模块，每个模块都有自己的作用域。在一个文件里面定义的变量、函数、类，都是私有的，对其他文件不可见。

> CommonJS规范规定，每个模块内部，module变量代表当前模块。这个变量是一个对象，它的**exports**属性（即**module.exports**）是对外的接口。加载某个模块，其实是加载该模块的**module.exports**属性。

* module.exports输出变量
* require方法用于加载模块。

### **CommonJS模块的特点如下**

> 所有代码都运行在模块作用域，不会污染全局作用域。
> 模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。
> 模块加载的顺序，按照其在代码中出现的顺序。

### module
> Node内部提供一个Module构建函数。所有模块都是Module的实例。
> 每个模块内部，都有一个module对象，代表当前模块

* 属性如下：
    - module.id 模块的识别符，通常是带有绝对路径的模块文件名。
    - module.filename 模块的文件名，带有绝对路径。
    - module.loaded 返回一个布尔值，表示模块是否已经完成加载。
    - module.parent 返回一个对象，表示调用该模块的模块。
    - module.children 返回一个数组，表示该模块要用到的其他模块。
    - module.exports 表示模块对外输出的值。

## node启动http服务
* **第一步** 引入HTTP模块
    `let http = require('http')`
* **第二步** 创建http服务

    ```js
        let app = http.createServer((req,res) => {
        	//console.log(req)
        	// 第一个参数是请求的所有信息
        	
        	//console.log(res)
        	//第二个参数是响应的方法
        	
        	res.write('返回给你数据')//返回给客户端数据 -- 有乱码
        	res.write('ok!')//不会覆盖之前返回的数据
        	res.end();//结束响应。否则title-icon位置会一直转圈
        })
    ```
    createServer方法 ---- 接受一个回调函数，该回调函数接受两个参数，
    
        参数1：客户端请求的所有信息
        参数2：保存的是响应的方法
        
    res.write('ok!') ----- 接收到请求后，服务器端返回给客户端的数据
    res.end(); ---- 结束响应
* **第三步** 定义监听的端口

    ```js
    app.listen(3000,()=>{
        console.log('开始监听')
    })
    ```
## node开启fs(file system)服务

```js
let fs = require('fs')//i添加内置fs模块
let http = require('http')//添加内置http模块

let app = http.createServer((req,res) => {//创建http服务
	if(req.url === '/index.html'){//判断请求的url地址
		fs.readFile('./src/home.html',(error,data)=>{
			if(error){
				console.log(error)
			}else{
				//console.log(data.toString('utf-8'))
				//在node界面打印出不是二进制的数据，如果文件中有编码格式，则不需要加
				
				res.write(data)//将读取的信息反馈给客户端
				res.end();//结束响应
			}
		})
	}
})

app.listen(3000,()=>{
	console.log('服务启动')
})
```
* let fs = require('fs')
    1. 引入fs（文件读取模块）
    
* fs.readFile( '目标文件路径',()=>{} )
    1. readFile接受两个参数
        - 参数1：要返回的内容文件所在的相对路径
        - 参数2：读取文件内容后执行的回调函数 --- 接受两个参数
            - 参数1：捕获的错误信息（**错误优先**）
            - 参数2：读取文件获取到的数据
* req.url ---- 请求的信息中有url信息

## express

```js
let express = require('express')//引入express模块是一个函数
let app = express();//函数返回一个对象

//路由
app.get('/',(req,res)=>{
	console.log('有人请求')
	res.send('给你了')
})

//监听
app.listen(3000,'localhost',()=>{
	console.log('可以请求了')
})
```

1. 路由

        路由是指如何定义应用的端点（URIs）以及如何响应客户端的请求。
        
        路由是由一个 URI、HTTP 请求（GET、POST等）和若干个句柄组成，它的结构如下： app.METHOD(path, [callback...], callback)， app 是 express 对象的一个实例， METHOD 是一个 HTTP 请求方法， path 是服务器上的路径， callback 是当路由匹配时要执行的函数。

    语法：
    > app.method(path,callback)
    
    * 路由路径：可以是字符串类型，也可以是带正则的字符串



2. 监听
        
    语法：
    > **app.listen( port [,ip ,callback] )**

    端口是必需的，第二个参数是

### response响应

* __dirname： 获得当前执行文件所在目录的完整目录名
* __filename： 获得当前执行文件的带有完整绝对路径的文件名

**sendFile(path)** --  向路径必须是绝对路径，否则不可行 可以使用 __dirname + 相对于当前文件的路径

### 中间件
> 中间件（Middleware） 是一个函数，它可以访问请求对象（request object (req)）, 响应对象（response object(res)）,和web应用中处于请求-响应循环流程中的中间件，一般被命名为 next 的变量。

```js
app.use(function(req,res,next){
	console.log('触发2')
	next()
})
```
语法：**obj.use(function(req,res,next){ event })**

* 接受三个参数
    1. 参数1：请求的信息对象 
    2. 参数2：响应的数据对象
    3. 参数3：用于通知下一个的函数 ---  web 应用中处于请求-响应循环流程中的中间件

* 中间件的功能包括：
    1. 执行任何代码。
    1. 修改请求和响应对象。
    1. 终结请求-响应循环。
    1. 调用堆栈中的下一个中间件。
* 如果当前中间件没有终结请求-响应循环，则必须调用next()方法将控制权交给下一个中间件，否则请求就会挂起。

> 就是从客户端请求过来一个数据req，在服务器响应数据以前，可以做提前处理，例如重定向等。
可以有多个中间件，依次执行，如果这个中间件处理完数据，必须通知下一个中间件开始处理，通过next()方法通知，
    
```js
let express = require('express')
let app = express();
//中间件做处理
app.use(function(req,res,next){
    //路径中有这些路径就会被跳转
	let addressArr = ['/a','/b']

	if(addressArr.includes(req.url)){
		if(true){
			res.redirect('/login')//重定向到/login指定返回的页面
		}else{
			next()
		}
	}else{
		next()
	}
})
app.use(function(req,res,next){
	console.log('触发2')
	next()
})

app.get('/',(req,res)=>{
	console.log('有人请求0')
	res.sendFile(__dirname + '/viewspage/home.html' )
})
app.get('/a',(req,res)=>{
	console.log('有人请求a')
	res.sendFile(__dirname + '/viewspage/a.html' )
})
app.get('/b',(req,res)=>{
	console.log('有人请求b')
	res.sendFile(__dirname + '/viewspage/b.html' )
})
app.get('/login',(req,res)=>{
	console.log('有人请求b')
	res.sendFile(__dirname + '/viewspage/login.html' )
})

app.listen(3000,'localhost',()=>{

	console.log('可以请求了')
})
```

### 静态文件目录
    
    在express中预留了一个属性static，用来定义静态文件目录
    定义：定义指定静态文件目录
    
> 图片css和js等静态文件都是存放在这个文件夹下，在index.html中引用这些静态文件的时候，
> 不是相对于这个html文件引用了，而是相对于定义的这个指定静态文件目录了，不用写

```js
app.use(express.static('./viewspage'))
```

### get请求

get请求在url中的？后面传输数据，express会通过req.query获取到？后面的数据，并把它转为一个对象。









