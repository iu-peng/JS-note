# React


---

## js文件
* react.development.js ----- 开发环境
* react-dom.development ----- 虚拟dom
* babel.min.js ----- babel转译器

> Babel 转义器会把 JSX 转换成一个名为 React.createElement() 的方法调用。

## 创建虚拟元素

语法：let h2 = React.createElement('h2',null,'hello')
参数1：元素标签名
参数2：元素属性
参数3：标签内容

### 在指定标签中渲染创建的虚拟元素
RaeactDom.render(h2,document.getElementById('box') )

### 使用jsx语法生成元素

**script标签的type必须是babel,这样就得引入babel的js文件，浏览器引擎不会主动解析type="text/balel“的js，让Babel引擎解析**

### 传值

```js
let str = '这是一句话'
let strArr = [1,2,3,4,5,6,7]
let liArr = strArr.map((item,index)=>{
    //循环的时候必须有唯一的key值，否则报错
	return <li key={index}>{item}</li>
})

ReactDOM.render(
	<ul>
		<li><h1>{str}</h1></li>
		<li><h1><a href="#">这是a标签</a></h1></li>
		{liArr}
	</ul>,
	document.getElementById('box')
)
```

## 组件
> 组件必须通过class继承React.Component，否则不行，在实例中定义render函数方法，return出一个由大标签包裹诸多元素的结构

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);

--------------等同于------------------

const element = React.createElement(
  'h1',//标签名
  {className: 'greeting'},//标签属性
  'Hello, world!'//标签内容
);
```

### **引用单个组件**

```js
//定义继承自React.Component的实例对象
class Haa extends React.Component{
	render(){
		return <div>
					<h1>这是大标题</h1>
					<ul>
						<li>dd</li>
						<li>dd</li>
						<li>dd</li>
					</ul>
				</div>
	}
}

ReactDOM.render(
	<Haa></Haa>,
	document.getElementById('box')
)
```
### **多个组件封装**

```js
class List extends React.Component{
	render(){
		return <ul>
					<li>1</li>
					<li>2</li>
					<li>3</li>
				</ul>
	}
}

class Haa extends React.Component{
	render(){
		return <div>
					<h1>这是大标题</h1>
					<List></List>
					<List></List>
				</div>
	}
}

ReactDOM.render(
	<div>
		<Haa></Haa>
		<Haa></Haa>
		<Haa></Haa>
	</div>,
	document.getElementById('box')
)
```

### **组件间的传值**
* 没有进行对象封装

```js
let title1 = '这是标题1'
let list1 = [1,2,3,4]

let title2 = '这是标题2'
let list2 = ['a','b']
//列表组件
class List extends React.Component{
	render(){
		return <ul>
		            //通过{}的形式可以直接渲染
					{
					    //接收List通过abc传递过来的参数，用this.props.的形式接收
						this.props.abc.map((item,index)=>{
							return <li key={index}>{item}</li>
						})
					}
				</ul>
	}
}
//一个封装的大组件 包括标题+列表
class Haa extends React.Component{
	render(){
		return <div>
					<h1>{this.props.title || '默认标题'}</h1>
					//接收Haa通过title传递过来的参数，用this.props.的形式接收
					<List abc={this.props.lis}></List>
					//接收Haa通过lis传递过来的参数，用this.props.的形式接收，然后再传给List实例，信号为abc
				</div>
	}
}
//总的结构渲染
ReactDOM.render(
	<div>
		<Haa title={title1} lis={list1}></Haa>
		//通过title属性把参数传给class定义的Haa实例，该实例通过Haa的title作为信号接收
		<Haa lis={list2}></Haa>
	</div>,
	document.getElementById('box')
)
```

### **进行对象封装**

```js
let obj1 = {
	title : '这是标题1',
	list : [1,2,3,4]
}
let obj2 = {
	title : '这是标题2',
	list : ['a','b']
}

class List extends React.Component{
	render(){
		return <ul>
					{
						this.props.abc.map((item,index)=>{
							return <li key={index}>{item}</li>
						})
					}
				</ul>
	}
}

class Haa extends React.Component{
	render(){
		console.log(this.props)
		return <div>
					<h1>{this.props.title || '默认标题'}</h1>
					//接收来自这个扩展运算符的数据，并提取相应的数据
					<List abc={this.props.list}></List>
					//把提取的数据传给相应的元素
				</div>
	}
}

ReactDOM.render(
	<div>
		<Haa {...obj1}></Haa>
		//不用写属性了，直接写一个扩展运算符传递给下面的元素
		//这和上面直接写属性的方式不同，它会把整个对象传过去
		<Haa {...obj2}></Haa>
	</div>,
	document.getElementById('box')
)
```
### **自定义标签类型**

```js
class Button extends React.Component{
	render(){
		let style = {width:'100px',height:'30px',background:'orange'}
		let node = <div style={style} >{this.props.title || '按钮'}</div>
		if(this.props.type === 'text'){
			node = <input type="text" />
		}else if(this.props.type === 'btn'){
			node = <input type="button" />
		}
		return node
	}
}

ReactDOM.render(
	<Button type="" />,
	document.getElementById('box')
)
```

## 生命周期
1. 构建阶段
* constructor
* componentWillMount -- 可以发送请求拿数据
* rander -- 渲染结构
* componentDidMount -- 此时可以获取元素

2. 数据更细
* shouldComponentUpdata -- 更新前判断是否进行更新，是否进入componentWillUpdate，可提高性能
* componentWillUpdate -- 更新前
* rander -- 渲染视图 **不允许在此处更新状态**
* componentDidUpdate -- 更新完成

3. 卸载阶段
* componentWillUnmount -- 

## 知识点

1.  JSX, 是一种 JavaScript 的语法扩展

2.  在 JSX 当中使用 JavaScript 表达式要包含在大括号里。

    ```js
    <ul>
    	{
    		this.props.arr.map((item,index)=>{
    			return <li key={index}>{item}</li>
    		})
    	}
    </ul>
    //this.props.arr是一个数组，通过map方法会返回一个数组，react会自动把它拆分解析成单个的，并进行解析，插入
    ```
3. 推荐在 JSX 代码的外面扩上一个小括号，这样可以防止 分号自动插入 的bug.

    ```js
    let str = <ul>
        <li>li标签</li>
    </ul>
    ---------这两种写法效果是一样的-------------
    let str = ( <ul>
        <li>li标签</li>
    </ul> )
    ```
4. 切记使用了大括号包裹的 JavaScript 表达式时就不要再到外面套引号了。JSX 会将引号当中的内容识别为字符串而不是表达式。

    ```js
    <li key={index}>{item}</li>
    -------下面这样的是被当做字符串的-----------
    <li key='{index}'>{item}</li>
    ```
    **大括号当中的js表达式是作为值的，如果再大括号外面加上引号，就会被当作固定的字符串**
    
5. 如果 JSX 标签是闭合式的，那么你需要在结尾处用 **/>**

    类似img，input这样的标签，再jsx中必须在标签末尾添加上结束符号**/**，否则完犊子

6. 因为 JSX 的特性更接近 JavaScript 而不是 HTML , 所以 React DOM 使用 **camelCase** 小驼峰命名 来定义属性的名称，而不是使用 HTML 的属性名称。例如，**class** 变成了 **className**，而 **tabindex** 则对应着 **tabIndex**.

7. **ReactDOM.render()** 的方法来将其渲染到页面上

    ```js
    const element = <h1>Hello, world</h1>;
    ReactDOM.render(
      element,
      document.getElementById('root')
    );
    //将定义好的元素通过这个方法渲染到页面上
    ```
8. **React 元素都是不可变**的。当元素被创建之后，你是无法改变其内容或属性的。一个元素就好像是动画里的一帧，它代表应用界面在某一时间点的样子。
    学到这里，更新界面的唯一办法是创建一个新的元素，然后将它传入 **ReactDOM.render()** 方法
    **new Date().toLocaleTimeString()**：把当前时间转为 下午4:31:57 这样的形式

9. 定义组件的方式 **组件的返回值只能有一个根元素**

    ```js
    function Welcome(props) {
      return <h1>Hello, {props.name}</h1>;
    }
    
    -----------这两种方式定义组件的结果是一样的---------
    
    class Welcome extends React.Component {
      render() {
        return <h1>Hello, {this.props.name}</h1>;
      }
    }
    ```
10. 当React遇到的元素是用户自定义的组件，它会将JSX属性作为单个对象传递给该组件,这个对象称之为“props”。

    ```js
    ReactDOM.render(
		<div>
			<Haa title={title1} lis={list1}></Haa>
			<Haa lis={list2}></Haa>
		</div>,
		document.getElementById('box')
	)
    ```
    再上面的标签中，React会将自定义组件的属性title作为对象传给该组件。在组件中通过props接收

11. 函数定义组件传值props的例子

    ```js
    function Fn(s){
		return <h2>你是：{s.name}</h2>
	}

	const ele = <Fn name="男生" />
	//组件名称必须以大写字母开头。
	ReactDOM.render(
		ele,
		box
	)
    ```
    **以上组件传值的步骤：**
    > 我们对<Fn name="男生" />元素调用了ReactDOM.render()方法。
    
    > React将{name: '男生'}作为props传入并调用Fn组件。
    
    > Fn组件将`<h2>你是：男生</h2>`元素作为结果返回。
    
    > React DOM将DOM更新为`<h1>你是：男生</h1>`。

12. React是非常灵活的，但它也有一个严格的规则：所有的React组件必须像纯函数那样使用它们的props。











