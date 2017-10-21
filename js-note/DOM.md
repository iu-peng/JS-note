# DOM 文档对象模型（document object model）

---

### JS的组成：

ECMAScript,DOM,BOM

----------
定义：通过ecma给的接口，获取页面中的标签，进而对干元素进行操作
### DOM节点
#### 1.元素节点
    每个HTML元素
#### 2.属性节点
    HTML元素的属性
#### 3.文本节点
    HTML元素内的文本
#### 4.注释节点
    注释<!---->
#### 5.文档节点
    整个文档document


----------


### 节点类型
|节点类型|元素节点|属性节点|文本节点|文档节点| 注释节点|
|:------:|  :------:  | :------:|:------:|:------:|:------:|
|方法nodeType|div.nT|div.attributes[0]||document.nT||
|节点值|1|2|3|9| 8|
|节点名nodeName|大写标签|属性名| \#text|\#document| \#comment|


----------


### 1  **获取**节点方法
* 获取节点名

        div.nodeName //DIV 
    
* 获取节点值

        div.nodeValue // 
        
* 获取所有**子节点**

        div.childNodes----获取div下所有的子节点，文本节点，元素节点，注释节点等
        
* 获取**一层子节点**

        div.children---- 获取该元素下所有的子标签节点，不会获取孙级的元素。

* 获取**父集节点**

        div.parentNode---- 获取div的上一级的父节点，只是一层
* 获取**兄弟节点**
1、相近的

        div.previousElementSibling----获取div的上一个兄弟节点
        div.nextElementSibling----获取div的下一个兄弟节点
2、第一个最后一个

        div.firstElementChild----获取div的第一个兄弟节点
        div.lastElementChild-----获取div的最后一个兄弟节点

### 2  **创建**节点方法
* document.createElement('li')
    
        固定写法：前面是document

### 3  在**元素后添加**一个节点方法
* element.appendChild(Li)

        类似push方法，并不会改变element原先的内容，只是在后面追加了内容
* 返回值是添加的元素

### 4  在元素指定**子节点前添加**一个节点方法
* element.insertBefore(NewElement,childNode)

        方法：在element的指定节点childNode前添加NewElement
        childNode可以不存在，但是NewElement节点必须存在，否则报错。
        childNode不存在时，功能相当于appendChild，会在element后添加
```
var newLi = document.createElement('li')
newLi.innerHTML = text.value;
list.insertBefore(newLi,null)
//在null前添加，父集中没有null，会在list的内容结构后添加
```
* NewElement节点必须存在
  NewElement和childNode必须是节点

### 5  **替换**指定的子节点
* element.replaceChild(NewElement,ChildNode)

        方法：用NewElement替换element元素下的指定节点ChildNode

* NewElement和ChildNode必须存在。且都是节点

### 6  从父集中**删除**指定子节点
* element.removeChild(childNodes)
    
        方法：直接把childNodes从父集element中删除
### 6.1 删除自己
* self.remove();

### 7  把自己的父集**删除**
* element.parentNode.remove()

        方法：直接把element的父集删除

### 8  克隆一个元素
* element.cloneNode(参数可选)

        默认只克隆element的节点，不包含element的子节点
        参数可选true和false
        没有参数：只克隆自己不克隆子节点
        参数为true: 克隆自己也克隆子节点
        参数为false： 只克隆自己不克隆子节点

* 只是克隆结构，不会克隆事件

##  属性操作
> 1.自定义属性是给对象添加属性，并不会添加在元素行间，因为获取到的元素是一个对象。
> 2.在行间以date-开头的自定义属性，获取一个元素之后，会放在dateset这个独立属性上，
设置时属性单词间用-连接，手动添加。js中手动改成驼峰式的。

* 设置行间属性1
        
        div.setAttribute(key,value)
        key和value都是字符串类型的，设置之后可以把属性添加到行间

* 设置行间属性 2
    
        data-index 用于设置行间属性
    ```
    <a data-index="${i}" href="javascript:;">x</a>
    ```
        dataset.index 获取调用属性时，用dataset.index
    ```
    obj[this.dataset.index] = null;
    ```
* 获取行间属性

        div.getAttribute(key)
        key是字符串类型
        可以获取到元素行内本有的属性。
        可以获取到通过setAttribute（）添加到元素的属性
        在行内手动添加的其他属性，也可以通过getAttribute（）获取到，但是不能通过`element.其他属性`获取到
        
        可以获取a标签的href属性的相对路径‘./images/1.jpg’，而不是绝对路径

* 删除行间属性

        div.removeAttribute(key)
        
## 图片懒加载
> 页面拉到什么位置才开始加载图片的思路：
img标签有两个属性，src和\_src,刚开始把正在加载的图片地址放在src中，把真实的图片地址放在\_src中，当页面拉到图片位置时，把图片的\_src的地址给到src中，就会显示真是图片，为防止回拉时再次加载，要在给的同时，把属性\_src删掉。

## 尺寸操作
### 可视区域的宽高
`document.documentElement.clientWidth`---可视区域的宽度
`document.documentElement.clientHeight`---可视区域的高度
> documentElement 指的是HTML元素

### 元素所占的宽高
`element.clientWidth` --- 获取元素所占的宽，不包括border，margin
> element.clientWidth = width+padding

`element.offsetWidth` --- 获取元素所占的宽，包含width,padding,border,不包括margin
> element.offsetWidth = width+padding+border

### 改变窗口大小获得尺寸触发事件
> 改变窗口大小的同时，更新窗口大小的数值
```
window.onresize = function(){
	console.log(document.documentElement.clientWidth)
	console.log(document.documentElement.clientHeight)
}
```

### 浏览器滚动条滚动触发事件
> 滚动滚动条或滑动滑轮的同时更新数据，配合页面滚动距离使用
```
window.onscroll = function(){ 

}
```
### 页面滚动的距离
> document.documentElement.scrollTop--**IE，火狐识别**
> document.body.scrollTop--**Chrome识别**

```
//Y轴：
document.documentElement.scrollTop
//IE 和 火狐识别此方法，在HTML身上
document.body.scrollTop
//Chrome识别，认为滚动在body身上

//X轴：
document.documentElement.scrollTop
//IE 和 火狐识别此方法，在灰HTML身上
document.body.scrollLeft
//Chrome识别，在body身上
```

### 内容高度 - scrollHeight
> 如果div1高度为100px，padding为20px中有另一个div2，里边的div2的高度为200px，则div.scrollHeight = 220px;因为scrollHeight包括除了border之外的padding和内容高度

### 定位父集-offsetParent
`element.offsetParent`
获取到最近的定位父集，没有默认为body

### 偏移量-offsetLeft
`offsetLeft` --- 相对于最近的定位父集的偏移量
`offsetTop` --- 相对于最近的定位父集的偏移量

### 相对浏览器可视区域的偏移量

> element.getBoundingClientRect();

> 浏览器的左边和上边相对于元素上右下左边的距离，包括border，padding。从border外开始算起

## 表单操作
### 1 获取所有的表单
> document.forms

 * 获取第一个表单
 > document.form[0]

### 2 获取表单控件
> document.form[0].name属性值

### 3获取form表单下select中的option的value值
> document.forms[1].selectedName.optionName.value;

> 逐渐获取document下的第二个表单，下面的selected标签，下面的option标签，属性value的属性值；

### 4 事件
* onchange
> 监控select等的改变，发生改变就触发事件

* oninput
> 监控input输入框的改变，改变值后移出光标才会触发事件

### 5 button添加提交事件

```javascript
button.onclick = function (){
    document.forms[0].submit();
}
//会让button按钮添加和submit提交按钮相同的功能。
```

### 5 button添加重置事件

```javascript
button.onclick = function (){
    document.forms[0].reset();
}
//会让button按钮添加和reset提交按钮相同的功能。
```
### 6 取消a标签的默认跳转行为
```
as.onclick = funciton (){
    return false;
}
```
### 7 取消submit标签的默认跳转行为
```javascript
sub.onsubmit = funciton (){
    return false;
}
```










