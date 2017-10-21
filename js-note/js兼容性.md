# js兼容性


---

## 1. getElementsByClassName
IE 低版本不支持

## 2. querySelector() ie不支持

## 3. trim()去前后空格的方法
IE低版本不支持

## 4. forEach()ie不支持

```js
if(!Array.property.forEach){
	Array.property.forEach = function(callback,orgThis){
		for(var i=0; i<this.length; i++){
			if(orgThis){
				callback.call(orgThis,this[i],i)
			}else{
				callback(this[i],i)
			}
		}
	}
}
```

## 5. firstElementChild  ie9下不支持

## firstChild
ie下返回的是第一个元素
高版本返回的是第一个子节点，ke'nen't

## 事件对象event

IE低版本全局定义了一个变量event，用来存事件对象
处理兼容 var e = ev || event

## addEventListener 和 attachEvent
IE低版本attachEvent,没有第三个参数，只有冒泡
detachEvent

attachEvent中的this指向window

```js
//改变this的指向
function fn(){  }
btn.attachEvent( 'onclick', function(){ fn.call(btn) } )
```

## ajax

IE下Ajax对象用 **new ActiveXObject('Microsoft.HTTP')**
IE6不支持

```js
//处理兼容
var xhr = null;
if(window.XMLHttpRequest){
    xhr = new XMLHttpRequest()
}else{
    xhr = new ActiveXObject('Microsoft.HTTP')
}
```

