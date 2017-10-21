# BOM



---

### 定义
    浏览器对象模型
### window.open('参数1','参数2','参数3');

> window.open('http://www.baidu.com','_self','height:500px,width:200px');

> 参数1规定：打开页面的地址链接--没有：默认打开空白页面
> 参数2规定打开页面的方式--没有：默认在新窗口打开；_blank
> 参数3规定打开后的页面样式

> window.opan()有返回值，返回的是新页面的window对象

#### 打开页面直接打开新窗口
window.open();
    火狐和Chrome 会做安全限制
    IE没有限制

#### 点击document打开新窗口
document.onclick = function(){
    window.open()
}

    在IE下要给代码添加条件window.onload = function(){代码}

### window.close();
> 一打开页面 就关闭页面

> 火狐和Chrome直接关闭，IE询问后关闭

> **可以在本窗口中关闭通过js方法open的新窗口(另一个窗口)**

通过document点击后关闭页面
> chrome直接关闭
> IE询问后关闭
> 火狐坑，只能在由window.open()打开的页面代码中添加window.close()关闭由window.open()打开的页面

### 用户代理信息
> window.navigator.userAgent

* chrome : 

        Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36

* 火狐：

        Mozilla/5.0 (Windows NT 6.1; WOW64; rv:52.0) Gecko/20100101 Firefox/52.0

* IE：版本不同，信息也不同
        
        Mozilla/5.0 (Windows NT 6.1; WOW64; Trident/7.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; .NET4.0C; .NET4.0E; rv:11.0) like Gecko

> 可以通过代理信息中查找浏览器特有字符判断浏览器类型 
> window.navigator.userAgent.indexOf('')


### 获取地址栏里的信息-window.location()

> window.location()
```
location.href:
http://www.example.com:8080/path/index.html?a=1&b=2#TOP
```
要获得URL各个部分的值，可以这么写：
```
location.protocol; // 'http'
location.host; // 'www.example.com'
location.port; // '8080'
location.pathname; // '/path/index.html'
location.search; // '?a=1&b=2'
location.hash; // 'TOP'
```

* 获取完整信息

        window.location().href

* 获取查询数据

        window.location().search
        地址栏中?到#间的内容
        改变search值后会刷新页面

* 获取hash值

        window.location().hash
        改变哈希值并不会刷新页面，用来定位浏览位置

### 获取浏览器窗口的内部宽度和高度
> 内部宽高是指除去菜单栏、工具栏、边框等占位元素后，用于显示网页的净宽高。

* window.innerWidth 获取浏览器窗口的内部宽度

* window.innerHeight 获取浏览器窗口的内部高度

* window.outerWidth 可以获取浏览器窗口的整个宽。

* window.outerHeight 可以获取浏览器窗口的整个高。

### 浏览器的信息
* navigator.appName：浏览器名称；
* navigator.appVersion：浏览器版本；
* navigator.language：浏览器设置的语言；
* navigator.platform：操作系统类型；
* navigator.userAgent：浏览器设定的User-Agent字符串

### 屏幕信息

* screen.width：屏幕宽度，以像素为单位；
* screen.height：屏幕高度，以像素为单位；
* screen.colorDepth：返回颜色位数，如8、16、24。

### 加载新页面
要加载一个新页面，可以调用location.assign()
### 重新加载当前页面
调用location.reload()





















