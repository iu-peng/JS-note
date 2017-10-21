## js基础知识



---
[TOC]

### 1.JS出现的位置
- #### 写在标签行间

   如： `<div onclick="alert(1)">测试</div>`
   
- #### 内嵌在页面中
    
    放在`head`中，或`body`标签将结束的位置。
    * 放在`head`中时，`script`标签中要添加
    
        ```
        window.onload = function (){ 代码块 }
        ```

    *  放在`body`中时则不需要，只是尽量把标签放在body标签将结束的位置。

- #### 通过外部引进`script`标签。如：
    ```
    <script src="./test.js"></script>
    ```
    **注意:script使用了src，写在这对script标签中的代码不执行**

### 2.通过js操作页面中的元素

- #### 通过id获取元素：仅有此一种方法。

    ```
    document.getElementById('id名字')
    ```
    **注意:id名字一定要加上双引号或者单引号**
- #### 通过ClassName获取元素，可获取到元素内的所有class元素。---->获取到的是一组元素。

    1、`document.getElementsByClassName('class');`
   
    获取的是一个集合，取某个元素时用下标。如aLI[2],从0开始。类似数组。

    2、在id元素下获取：
    
    `oList.getElementsByClassName('class');`

    3、在class元素下获取：
    
    `var aLi = document.getElementsByClassName('class')[0]`
    
    `var aLis = aLi.getElementsByClassName('class');`
    
    或
    
    `var aLi = document.getElementsByClassName('class')；`
    
    `var aLis = aLi[0].getElementsByClassName('class');`

     **注意: class获取时，填写下标的时候，可以获取的时候直接填写，或者上级元素填写下标都可以。**
     
- #### 通过标签名获取,可获取到元素内的所有该元素。---------->获取到的是一组元素。

   ` document.getElementsByTagName('p');`
   
    获取的是一个集合，取某个元素时用下标。如aLI[2],从0开始。类似数组。
    
    2、在id元素下获取：
    
    `oList.getElementsByTagName('li');`
    
    （3）在class元素下获取：
    
    `var aLi = document.getElementsByClassName('class')[0]`
    
    `var aLis = aLi.getElementsByTagName('li');`
    
    或
    
    `var aLi = document.getElementsByClassName('class')；`
    
   ` var aLis = aLi[0].getElementsByTagName('li');`

    **注意：这种方法时动态获取的，如果从上往下解析时没有找到元素，在下面动态生成后会更新数据，和ByClassName类似。但是querySelector和querySelectorAll则不然，它只会在动态生成后才能获取到，而不会生成后更新。**

- #### 通过css选择器获取，分两种；
    
    * 1、querySelector
    
    ```
    var oP = document.querySelector('css选择器');
    ```
    
    获取的是当前文档中的第一个符合条件的元素。如：
    
    ```
    var oLi = document.querySelector('.list li');
    ```
    
    获取的是.list 元素写的第一个li。---------->获取到的是一个元素。

    * 2、querySelectorAll
    
    ```
    var oP = document.querySelectorAll('css选择器');
    ```
    获取的是当前文档中的所有符合条件的元素。如：
    ```
    var oLi = document.querySelectorAll('.list li');
    ```
    获取的是.list 元素下的所有的 li。---------->获取到的是一组元素。

### 3.代码注释

    // 单行注释 注释其实就是说明一段代码
    
    /*
	    多行注释
    */
    
### 4.调试代码方法   

    alert()

    console.log()
    
    console.dir() 打印元素对象（包含属性）
    
### 5.变量

>变量：存放一些数据的，要使用变量之前，先要声明变量.
    
>var 变量名; 声明了变量未赋值，默认的值为undefined

>var 变量名 = 数据/值;  声明了变量并且赋值

>只是声明变量就alert该变量，会得到一个默认值undefined;

>未声明变量就alert该变量，会报错；...is not defined
    
    知识点1： 一条语句结束了 尽量每句结束位置都添加，函数和for或if结束不用添加。

    知识点2： = 赋值的意思 把等号右边的值赋给左边
    
#### 5.1.变量的命名规则

- 组成：字母 、 数字、 _  、 $ 组合而成
    
- 变量名不能已数字开头

- 不能是关键字和保留字
    
- 保留字:js即将使用的单词

    >关键字：js正在使用的单词 var function for if break continue class ...
    
#### 5.2.变量的命名风格：驼峰命名法

    大驼峰 多个单词组合的时候，从第一单词开始首字母大写
    
    小驼峰 多个单词组合的时候，从第二单词开始首字母大写
    
### 6 字符串的拼接方法

**6.1  字符串拼接的方法：**
    
> 1、把复制到单引号的内容进行规整，回车一定要去掉，不然会报错。

> 2、如果不去掉回车的话，解决办法是给每行当做字符串添加单引号，将多行用+连接起来；

> 3、不回车的第二种解决办法：不用给每行单独添加单引号，保持回车不取消，在每一行结束的位置添加转义字符‘\’，这样每行结束的回车就会被消除，整体还是用单引号包住，最后一行不用加 \。
    
**6.2  带数字的拼接方法：**

> 1、两边是数字，做加法运算，得到结果。

> 2、一边是字符串不管另一边是什么，都会做字符串拼接，转化成字符串。

> 3、如果是‘miaov’+1+2+‘ketang’这样的，出来的结果就是miaov12ketang;
因为从左往右解析，‘miaov’+1会解析成字符串miaov1，‘miaov1’+2会解析成字符串‘miaov12’，所以出来的结果就是miaov12ketang。

> 4、如果是1+2+‘miaov’；出来的结果就是字符串3miaov。
因为从左往右解析，前两个数字会做相加运算，得到数字3,3+'miaov'会得到字符串‘3miaov’。

> 5、如果想从'miaov'+1+2+'ketang',得到miaov3ketang的结果，需要把中间做相加运算的数字用括号括起来，即：'miaov'+(1+2)+'ketang'。提高优先级。

> **如果一个数等于这个数加上另一个数，则可以简写成a=a+b-->a+=b;**

### 7 数组

> 数组：数组 类似于有顺序的仓库

- 定义数组，直接量定义数组 `var arr = []`

- 存值：存的值以逗号隔开，最后一个省略逗号

- 取值：下标/索引值  是一个数字,下标是从0开始的，依次数数,数组[下标]。

        本来应该是arr.n，但是变量名不能是数字开头，所以只能用带[]的命名法；
    
- 数组的长度：（数组中存了多少个值）数组.length  

- 数组的第一个值 arr[0]

- 数组的最后一个值 arr[arr.length-1]

### 8 取模

> 语法：a%b; 

- 如果a>b; a%b的结果就是a/b的余数。
- 如果a<b；a%b的结果就是a。

**向下取整：**

    0.1,0.9，都会取0,
    
    5.5,5.9 都会取5
>语法：Math.floor(0.9)--->0   Math.floor(5/9)--->1

### 9 for 循环

> 语法：for(var i=o; i<50; i++ ) {  }

    先定义一个值，给定判断条件，对定义的值进行运算。
    
    每次进入for循环，都会先进行第二步，
    
    判断是否满足条件，满足则进行循环内的操作，
    
    执行完/对定义的i运算一次。i++ 是在执行完i<50 之后进行。
    
    如果其中包含事件函数时，每次执行的时候会跳过这个函数，不执行函数，但是for循环已经执行完了i次。
**注意：括号内必须要有两个分号。**

> break：执行到某一步时跳出for循环。

> continue：跳过本次循环。不会终止，然后继续下次循环。

#### 9.1 for循环套for循环
```
 for( var i=1; i<10; i++ ){
 	str += '<div>'
 	for( var j=1; j<=i; j++ ){
 		str += '<span>'+j+'x'+i+'='+j*i+'</span>';
 	}
 	str+='</div>'
 }
```

> 最外层for循环是在每次循环执行完后才进行第二次循环。嵌套一个for循环时，在嵌套的整个for循环执行完后才进行第二次循环。
字符串中在for循环后插入另一段字符串的方法：事先把字符串劈开，从前往后添加。
第一次for循环时，定义的str如果是 `str+='<div></div>'`.那么进行中间嵌套的for循环时，如果想在str中间插入`'<span>'+j+'x'+i+'='+j*i+'</span>'`是不可行的，解决办法是把之前的`'<div></div>'`劈开，先定义前一部分，执行嵌套的for循环时再添加一部分，到外层一次for循环结束时再添加一部分。

#### 9.2 for循环中函数变量与for中变量不协调

for循环中嵌套一个别的函数或者嵌套一个不是for循环的东西时，函数中的变量i指的是外层for循环执行完之后的i的取值，相当于函数调用的是for循环之外的值，是一个固定的值。
```
for( var i = 0;i<4;i++){
	aBtn[i].onclick = function(){
		console.log(i)
	}
}
```
> 以上`console.log(i)`打印出的i是一个4，并不是for循环每次执行的i值，因为函数执行时，函数内的i调用的是for循环之后i的取值。如果其中包含事件函数时，每次执行的时候会跳过这个函数，不执行函数，但是for循环已经执行完了i次。

### 10 this

> this是个已经定义好的变量，this的值是不固定的，根据执行环境决定this的值。

**1、在script标签中，this的值为window**
    
    在一对script标签中定义的变量和函数，都会作为window的属性或方法，如：
	var abc = 123;
	console.log(this)
    this打印出的是window，在js中window可以不用写，
    如：
    console.log(this.abc)或者console.log(window.abc);
    
    
**2、this出现在函数中，this的值取决于谁调用该函数。**

在文档加载后直接调用的函数，this指向window；
    
```
function fn(){
	console.log(this)
}
```
	
打印出的是window这个对象（包含多个属性）；
    在事件后调用函数的话，this指向触发事件的元素标签
    
	aBtn[i].onclick = function(){
		console.log(this)
	}
	
打印出的是aBtn[i]

### 11 自定义属性

> 给元素添加本身没有的属性，如：btn.abc=1; 给btn添加自定义属性abc=1;

> 用在判断跟自己有关的状态的情况，如果多个元素公用一个状态会出现混乱的情况，这种情况下不再适合定义一个公用的开关，需要给每个元素自定义一个开关。

    属性分为系统定义好的属性和自定义属性。
    
**模拟多选：**

```
for( var i=0; i<aBtn.length; i++ ){ //每次按钮点击用for循环
	aBtn[i].onOff = false; //给每个按钮添加自定义属性onOff，
	aBtn[i].onclick = function(){ //每一次点击的效果
		if(this.onOff){
			this.style.backgroundColor = '';
			this.onOff = false;
		}else{
			this.style.backgroundColor = '#20b2aa';
			this.onOff = true;
		}
	}
}
```

### 12 字符串

> es6提供的反引号``：模板字符串（超级字符串）

    在反引号中间直接将需要粘贴的html结构粘贴进来，不需要进行字符串拼接，不需要担心回车换行的影响。
    
    在超级字符串中添加变量的时候，不能用传统的方法劈开加变量了，只能用${变量名}这样的占位符添加变量
    
**字符串一旦定义，就不能更改，可以改掉变量存的值，但是原本的字符串并没有改变，只能读并不能改。数组则可以。简单类型的是赋值，数组等对象的是赋址，改变的是对象的地址.**

### 13 运算符

> **算术运算符**：+、-、*、/、%、++、--

> **赋值运算符**：=、+=、-=、*=、/=、%=

> **关系运算符**：<、>、<=、>=、==、！=、===、！==

    console.log( 5<2<3 ) 返回的值都是true。因为这个运算步骤是从左往右进行的，
    首先5<2，转为false，然后false<3,会把false转为0，然后0<3,就是true。


**两个==**

    console.log( 1=='1') 会发生隐式类型转换，把字符串转为数字再进行比较。为true。

**三个===**

    console.log( 1==='1') 不会发生隐式类型转换，只要类型不同就为false，更高效。 

**！=**

    console.log(2!='2') 先进行类型转换，转为数字后进行比较，跟两个==的步骤类似

**！==**

    console.log( 2!=='2' ) 不会发生类型转换，只要类型不同，就进行判断。

> **逻辑运算符**：&& 、 || 、 !

**1、&&：且/and**

    左右两边得同时满足；并且的意思。
    
**2、||：或/or**

    有一个满足就行，或者的意思
    
**3、！：取反**

    取对立面
    
**4、三目运算符**

> 判断条件 ? 条件满足执行的代码 : 不满足执行的代码

```
var n = 0;
var m = n == 0? 1: 'abc';
```

先执行三目运算符,n==0满足，取到值1，然后将1赋值给m,——m=1;

**不要嵌套太多三目运算符。**
**5、&&、||、？ ：**
> **&&**：左边不成立，不管右边是否为true，都不执行，直接返回左边的结果。左边成立，直接返回右边的结果。
> **||** : 左边成立，直接返回左边的值；左边不成立，直接返回右边的值。

### 14 函数

> 可存放多行代码，形成代码块。是一个对象object，可执行，可以作为值使用，

#### 14.1 语法

```
function [函数名](参数1，参数2，参数3，···){
    函数体 代码块
    [return 值] //可写可不写
}
```
#### 14.2 函数定义：函数代码不会自动执行。必须通过函数调用。
    
    函数声明：函数声明必须带上名字。否则报错
    
  function 函数名(){  }


    函数表达式：把函数作为值使用，赋值给变量
    var a = function 函数名{  }// 函数名可写可不写，有函数名但是函数名为a.

#### 14.3 函数调用：函数定义后，不会执行函数中的代码，只能调用后才能执行。

**声明后调用**  

    函数名（）；————直接调用。可以调用多次

**声明前调用**    

    函数名（）；————直接调用。可以调用多次


**函数表达式的调用：**

    必须在函数表达式之后调用，之前的话报错。

#### 14.4 函数参数：

> 在函数公用的时候，在调用相同函数的时候要执行不同的值，则要通过函数传参的方式。

**形参：**

    函数定义的时候，写在小括号中，形参的命名规则，要符合变量的命名规则。
    
    形参只能是函数内部声明的变量。默认值为undefined。形参只能在函数中使用。
    
> function fn([形参1,形参2,形参3]){  }

**实参：**

    在调用函数时，写在小括号中的参数。用形参接收这个实参。
    
    形参和实参是一一对应的，以逗号隔开。
    
#### 14.5 arguments：

> 实参的个数不固定的时候，使用函数内部的内置变量：**arguments**代表实参的集合，类数组。
    
**类数组和数组的相同点：**

     结构类似[]; 有length； 有下标；

**不同点：**

     类数组没有数组有的方法：如数组arr.push(1);可以执行，但是类数组arguments.push(1);则会undefined。

#### 14.6 函数的返回值：return

> 1、每个函数执行完后都会有一个结果。默认为undefined
> 2、函数的返回结果，return后面写的是什么，返回值就是什么，如果没有return，默认返回值是undefined。
> 3、使用return，只能在函数中使用，函数调用后出来return后面的值，如果return后没有值，函数出来的是undefined。return之后的代码停止执行，终止后面的代码执行。 

### 15 JS中的数据类型

> **number， string， boolean ，Null，对象(object)，Undefine。**

> **基本（简单）数据类型**：数字(number)，字符串(string)，布尔值(boolean)，Undefined(未定义），Null(空）。

> **复合（引用）数据类型**：对象（0bject）。

#### 15.1 字符串 String

    一对单引号或双引号包含的0个或多个字符串组成的串。
> **有内置函数String()**

**input 中的输入框中的value都是string；**

#### 15.2 数字 Number

> 包含整数和浮点数（小数）。

    正无穷大：Number.MAX_VALUE——js中表示最大的数。——+Infinity
    
    负无穷大：Number.MIN_VALUE——js中表示最小的数。——-Infinity
    
> **内置函数：Number()**

#### 15.3 布尔值 Boolean

> true或false

> **内置函数Boolean()**;

#### 15.4 Undefined
> 类型为首字母大写，值为首字母小写

> 值：undefined；

**出现情况**

    1、定义的变量未赋值；
    
    2、找一个对象未存在的属性。
    
**无内置函数**

#### 15.5 Null：值null

> Null：值null

`var str=null;`

**无内置函数**

#### 15.6 对象 object

    是由0个或多个名/值组成的集合如`{名：值，名：值}`,
    
    object包括window，document，数组，json，函数（特殊的可执行的对象，js中属一等公民）。

> 内置函数object

### 16 JS中的数据类型转换

    常用的为转Number，String，Boolean. 显式类型转换
    
#### 16.1 转成数字类型

**(1)Number(a)：将其他类型转成数字**

    转不了的显示NaN(not a number)，如数组中有多个数字或函数或空的json或未定义的东西

> NaN是一个不是数字的数字类型。在布尔值中是false。出现在程序中，说明进行了非法操作。NaN和任何值包括自己都不相等。和任何值比较返回false。

> null，空字符串，空数组，空格，布尔值false会转为0

**出现NaN的情况：**

    1、调用Number之后，不能转对应的数字，会显示NaN。
    
    2、进行非法运算后，会用NaN显示。

**(2)parseInt(a,10):**

> 将字符串从左往右的拾取出其中的数字转化为整型数字，如果碰到一个不是数字的则终止。

    如parseInt('+100px')，出来的结果是100，即使后面还有数字，也不会继续了。
    
    +-号，空格都认，空格自动忽略。但是不认小数点。10代表按十进制来转。

> 简便方法：

```
    console.log((20/4.1))  //4.878048780487806
	console.log((20/4.1)|0)//取整型数类似parseInt --4
	console.log(~~(20/4.1))//取整型数类似parseInt --4
```

**(3)parseFloat('12.34px')：**


> 将字符串从左往右的拾取出其中的数字转为浮点数(小数)，一个小数点可以，如果有两个小数点，则只认第一个。

    parseInt('100.58px')和parseFloat('100.58px')结果是不同的，分别为100和100.58

**(4)isNaN(5)：**

> 转出为false;先通过Number()转出后，如果为数字，则isNaN()转化后为false;如果不是数字，则转化后为true.

	   NaN是一个不是数字的数字类型，所以它跟数字是相对立的，
	    isNaN()就是一个判断的方法，如果括号内的为数字，它就排斥，为false.


***

### 17 JS中的截取字符串slice
`string string.slice(参数1，参数2);`
返回截取后的值，就是截取后拿出该值。接收负值；
字符串.slice(2);
> 作用：截取字符串，从第3个字符开始位置开始截取，直到字符串末尾。

参数：
1. 没有参数返回整个字符串---slice（）
2. 如果参数为一个，则从该下表字符开始一直截取到末尾。----slice（2）
3. 如果参数为两个，则从第一个下标字符开始截取，直到第二个参数下标开始。----slice（2,4）

4. 参数1>参数2，返回空
5. 字符串.slice(-1)：代表最后一位，字符串从后往前推是从-1开始到-length；

### 18 JS中的截取字符串substring()
`string string.substring(参数1，参数2);`
> 返回截取后的值，就是截取后拿出该值。不接收负值；如果为负值，则从0开始

字符串.substring(2);
> 作用：截取字符串，从第3个字符开始位置开始截取，直到字符串末尾。

参数：
1. 没有参数返回整个字符串---substring（）
2. 如果参数为一个，则从该下表字符开始一直截取到末尾。----substring（2）
3. 如果参数为两个，则从第一个下标字符开始截取，直到第二个参数下标开始。----substring（2,4）
4. 参数1>参数2，调换位置后，重新截取，
5. 字符串.substring(-1)：从开始位置截取；
6. 超出字符长度，返回空。

### 19 substr();截取指定长度的值
`string string.substr(参数1，参数2)`
> 参数1指定开始截取位置，

* 没有的话，截取整个字符串
* 字符串.slice(-1)：代表最后一位，字符串从后往前推是从-1开始到-length

> 参数2指定截取长度。

### 20 split();
`string string.split(指定字符)`
> 通过一个指定字符串把元字符串分割组成一个数组

* 没有指定字符或没有的字符，会把整个字符组成一个数组
* 空字符，会把原字符串全部分割成单个字符组成数组。

### 20 toUpperCase();转为大写
### 20 toLowerCase();转为大写
### 21 charAt();
`string string.charAt(3)`
> 返回字符串 返回第几位的字符
> 没有参数默认为0位
> 超过string.length-1 返回空字符串。

### 22 JS中的indexOf
`number string.indexOf(searchvalue，参数2)`

字符串.indexOf(字符串1)
作用：**查找字符串1在字符串中首次出现的位置。**
如果有：返回出现位置开始的下标
如果没有搜索到：返回-1；

参数2指定开始搜索位置的下标，可选

### 23 JS中的lastIndexOf
`number string.indexOf(searchvalue，参数2)`

字符串.indexOf(字符串1)
作用：**从后往前查找字符串1在字符串中首次出现的位置。**
如果有：返回出现位置开始的下标
如果没有搜索到：返回-1；

参数2指定开始搜索位置的下标，可选
    
### 24 JS中的去除字符串前后空格trim();
字符串.trim（）
作用：去除字符串前后空格，中间有不起作用。



### 25 字符串解析成代码运行
> eval(aBox.value)

```
<textarea id="code"></textarea>
<input type="button" id="btn" value="运行代码" />
<script>
	btn.onclick = function (){
	eval(code.value)  // 字符串 解析成代码运行	
			}
</script>
```
## 数组的方法
> 数组方法中的回调函数中参数2都可以改变回调函数中的this的指向。

### 1 循环数组forEach
#### 使用方法：对应的值和找到下标。
缺点：
1. forEach不能选择从哪个位置开始循环。
2. 不能停止break和跳过continue
```
//语法
arr.forEach(function(item,index,array){
	console.log(item,index,array)
},可选参数2)
```
通过参数2可以改变forEach中this的指向
#### 自己封装：
```
var arr = [1,2,3,4,5,6];
function foreach(arry,callback){
	for(var i=0; i<arr.length; i++){
		callback(arry[i],i)
	}
}
foreach(arr,function(a,b){
	console.log(a,b);
})
```

### 2 将数组转为字符串arr.join(参数);
和split相反，没有参数时，默认用逗号连接，可以定义连接符，
参数定义连接符。
### 3 arr.push(参数1，参数2，··· )
> **向数组的末尾添加内容。会改变数组的内容**
> 返回操作之后的长度

push的返回值是数组添加内容之后的长度。
### 4 arr.unshift(参数1，参数2，··· )
> **向数组的开头位置添加内容。会改变数组的内容**
> 返回操作之后的长度

unshift的返回值是数组添加内容之后的长度。

### 5 arr.pop()
> **删除数组的末尾一项,没有参数**
> 返回数组中删除的内容

默认删除**最后**一个，每次调用都会删除一项。返回值是删除的那一项。

### 6 arr.shift()
> **删除数组的开始一项,没有参数**
> 返回数组中删除的内容

默认删除**开始**的一项，每次调用都会删除一项。返回值是删除的那一项。

### 7 arr.map(回调函数)
该方法会把回调函数执行结果拼成一个数组。返回该数组
```
var maps = arr.map(function(item){
	return `<li>${item}</li>`
//返回的是一个数组，将每一次执行完回调函数的值添加到这个数组的对应的位置
	})
	//list.innerHTML = maps;结构中都是字符串类型的，即使maps是数组，也会把它转为字符串，默认调用toString()方法，把逗号也转了，所以不能直接用。
list.innerHTML = maps.join('');
```


### 8 arr.filter(回调函数)
回调函数的返回值为true，那么就把循环到的这一项放到新数组中，在回调函数中，放入条件return 条件；将满足条件的项重新组成一个新的数组。并返回。
```
var arred = arr.filter(function(item){
	return item.age>25;
})
```
### 9 arr.concat();连接一个多个数组
```
var arr1 = [1,2,3]
var arr2 = [4,5,6]
var newarr = arr1.concat(arr2,['a','b'],'miaov')
// [1, 2, 3, 4, 5, 6, "a", "b", "miaov"]
```
括号中没有参数时，返回的是arr1，但是不是arr1，而是一个新数组

### 10 arr.reverse();颠倒数组，翻转数组
```
var arr3 = [1,2,3,4]
console.log(arr3.reverse())
//[4, 3, 2, 1]
```
    会改变原来的数组顺序。不接受任何参数。

### 11 arr.slice(参数1，参数2);截取指定位置的数组

```
var arr4 = [1,2,3,4]
console.log(arr4.slice())//[1, 2, 3, 4]
console.log(arr4.slice(1,3))//[2, 3]
console.log(arr4.slice(2))//[3, 4]
console.log(arr4.slice(-1))//[4]
console.log(arr4)//[1, 2, 3, 4]

```

没有参数时，默认截取整个数组，从头截到尾。
参数1定义开始位置，参数2定义结束位置前的。
接收负数，arr4.slice（-1），截取的是最后一个。
只有一个参数时，从该位置截取到结尾

### 12 arr.splice(参数1，参数2,参数3);截取指定个数后组成数组
参数1开始位置，参数2截取个数，参数3是替换的值；可以为多个

    会改变原来数组的值，分情况：1参数为空 2参数正常两个 3参数为一个 4参数为三个
```
	var arr5 = [1,2,3,4,5,6]
	
	console.log(arr5.splice())
	//为空数组
	console.log(arr5)
	//[1, 2, 3, 4, 5, 6]    返回的新的数组，和原来相似
	
	console.log(arr5.splice(2,3))
	//[3, 4, 5]
	console.log(arr5)
	// [1, 2, 6]    会改变原数组
	
	console.log(arr5.splice(2))
	//[3, 4, 5, 6]  一个参数从指定位置截取到结尾位置
	console.log(arr5)
	//[1, 2]
	
	
	console.log(arr5.splice(2,3,'a','b'))
	//[3, 4, 5]     替换
	console.log(arr5)
	//[1, 2, "a", "b", 6]   替换
```

### 静态
自定义的属性，挂在函数上的属性，属于静态属性
给函数属性定义一个函数，属于静态方法

### 13 数组的静态方法from，isArray

* 将类数组（可循环对象）转为数组Array.from(类数组)
```
var lis = document.getElementsByTagName('li')//lis为类数组
var lisarr = Array.from(lis)//转为数组
lisarr.push(1)
console.log(lisarr)     //[li, li, li, li, li, 1]

console.log(Array.isArray(lisarr))      //true
```
第二个参数时一个回调函数，将类数组的每一项做进一步处理，将回调函数中return后的值放入新生成的数组中。
第三个参数改变函数this的指向。

        转完之后可以调用数组的所有方法
    
* 判断类型是否为数组Array.isArray(值)

## for in循环
每一次循环的时候都会把对象的key值赋值给变量
`for(var attr in obj){}//attr都是字符串`

    数组的的key值是类型为字符串的下标。循环数组就用for循环，循环对象就用for in。也可以用for循环，但是key值必须是数字从0开始。手动添加length属性。

对象没有length属性，需手动添加。









