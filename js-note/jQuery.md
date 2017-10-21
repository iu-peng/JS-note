# jQuery



---
# 开始工作
## $(callback)和window.onload的区别
> 前者是所有的元素加载完后触发回调，后者是标签和资源全部加载完

`$(document).function(){}和$().function(){}一样`

原生的js元素不能直接使用jq方法，需要转为jq对象
jq也不能直接使用原生的方法，需转为原生的

* 原生对象转为jq对象

```js
let inputs = document.getElementsByTagName('input')
//直接通过$()就可以转为jq对象
$(inputs).click(function(){
    $(this).addClass('red')
})
```

* jq对象转为原生DOM

```js
 $('input').click(function(){
    // 此处的this指的是原生的this,要用jq的方法，就要转为jq对象
    $(this).addClass('red')
    // this.classList.add('red')
})
```
```js
let jqBox = $('div')
//通过jq对象[下标]的形式可以获取到原生DOM
console.log(jqBox[0])
console.log(jqBox.get(0))
```

### jq实现选项卡切换

```js
$(function(){
    $('input').click(function(){
        $('input').removeClass('red')
        $(this).addClass('red')

        let n = $('input').index(this)
        $('div').hide()
        $('div').eq(n).show()
    })
})
```
### jq添加类似addEventListener事件
> 第一个参数是事件名
第二个参数是指定选择器可以冒泡到事件元素，例子中只有document下class=active的元素和该元素下的子孙元素可以冒泡到document
第三个参数是传递的参数
第四个参数是事件函数

```js
$(document).on('click mouseenter','.active',{a:100},function(ev){
    console.log(22)
    if(ev.type === 'click'){
        console.log('click')

    }else if(ev.type === 'mouseenter'){
        console.log('mouseenter')
    }
})
```

# 效果
## animate--自定义动画
> 语法：$(selector).animate({params},speed,callback);

> parans:是一个包含多个属性+属性值的对象，--必需的参数
> speed:定义效果的时长，可选参数：slow/fast/毫秒
> callback:动画执行完后需要执行的函数名称

    属性操作：属性对象几乎可以操作任意css属性，但是除了颜色的改变

1. 操作多个属性
    parans对象：可以是多个属性，如：

    ```js
    {
        width:'100px',
        height:'200px',
        opacity:'0.2'
    }
    ```
2. 属性值使用相对值
    parans对象中的属性值除了可以是固定值外，还可以使用类似**+=，-=**这样的相对值,如：

    ```js
     {
        width:'+=100px',
        height:'-=200px',
        opacity:'0.2'
    }
    ```
3. 属性值使用预定义的值
    parans对象中的属性值除了以上两种形式，还可以使用**toggle,show,hide**这样的预定义值，如：

    ```js
    $('button').click(function(){
        $('div').animate({
            width:'toggle'//这样的设置会让width在有和无之间切换
        })
    })
    ```
4. 动画队列的实现
    通过多个animate并列在一起，就可以实现动画的按序播放，第一个运动完成后，紧接着运动第二个动画，依次进行。如：

    ```js
    $("button").click(function(){
    
        var div=$("div");
        div.animate({height:'300px',opacity:'0.4'},"slow");
        div.animate({width:'300px',opacity:'0.8'},"slow");
        div.animate({height:'100px',opacity:'0.4'},"slow");
        div.animate({width:'100px',opacity:'0.8'},"slow");
        
    });
    ```
## stop()--停止效果
> 定义：是用来动画或效果完成之前，停止它们。并不仅仅是用来停止动画的，还可以停止效果
> 语法：(selector).stop(stopAll,goToEnd);

> stopAll：规定是否清除动画队列，默认为false，意思就是不清除后面的动画，如第一个例子，点击一次停止后，只是清除了第一个height的动画，后面的动画队列依旧会运行。为true的话，则是清除所有动画队列。

> goToEnd：是否立即完成当前动画，默认为false，即会清除当前动画，并不会运行完当前动画，为true的话，则是把当前动画运行完，注意只是运行完当前动画，如果第一个参数设为false，后面的动画还是会执行。

1. 无参数--stop()
只是清除了当前的动画，并不会立即完成，后面队列中的动画依旧还在，还会运行。

    ```js
    $("button").click(function(){
        var div=$("div");
        div.animate({height:'300px',opacity:'0.4'},"slow");
        div.animate({width:'300px',opacity:'0.8'},"slow");
        div.animate({height:'100px',opacity:'0.4'},"slow");
        div.animate({width:'100px',opacity:'0.8'},"slow");
        
    });
    
    $("#stop").click(function(){
        $("div").stop();
    });
    ```
2. 第一个参数为true--stop(true)
会停止后续的动画队列，把动画暂停到某一状态，后续还可以在这个基础上进行

    ```js
    $("#stop").click(function(){
        $("div").stop(true);//只是暂停了所有的动画，以后还可以在暂停的基础上进行
    });
    ```
    
3. 第二个参数为true--stop(false,true)
点击会立即完成当前的动画，第一个参数为false，就是不会清除后续动画，还可以运行后面的动画，再次点击还是会清除第二个动画，并立即完成。

    ```js
    $("#stop").click(function(){
    $("div").stop(false,true);
        //让当前动画立即完成，后续的动画紧接着执行，再次点击会立即完成第二个。依次进行。
    });
    ```
4. 第二个参数为true--stop(true,true)
点击会立即完成当前动画，并且后续的动画，也会清除，但是还是会存在，下次点击还是会在这个基础上进行。

    ```js
    $("#stop").click(function(){
    $("div").stop(false,true);
        //让当前动画立即完成，后续的动画也会暂停。
    });
    ```
5. 停止效果
并不仅仅是用来停止动画。

```js
$(document).ready(function(){
    $("#flip").click(function(){
        $("#panel").slideDown(5000);//添加滑动效果
    });
    $("#stop").click(function(){
        $("#panel").stop();//停止当前元素的滑动效果
    });
});
```
## jQuery执行链
> 链允许我们在同一个元素上执行完一个命令后紧接着执行另一个动作，就不用给一个元素多次添加事件了。例如上面的动画例子

```js
$('div').animate({height:'300px'}).animate({width:'300px'}).animate({height:'100px'});
//这样书写会让同一个div元素依次执行三个动画动作，不用每次都添加了
```
# HTML
## DOM操作--获取并设置改变内容
* text()---设置或返回所选元素的文本内容
* html()---设置或返回所选元素的内容（包括 HTML 标记）
* val()---设置或返回表单字段的值

**括号中没有内容时，就是获取内容。括号中有内容时，就是设置内容的值**

> text() 设置或返回所选元素的文本内容

```js
<p id="test">这是段落中的<b>粗体</b> 文本。</p>

$("#btn1").click(function(){
    alert("Text: " + $("#test").text());//只是获取p标签的内容，不包含其中的标签。
    //Text: 这是段落中的粗体文本。
});

//-------------设置内容----------------
$("#btn1").click(function(){
    $("#test").text('改变了段落内容')
    alert("Text: " + $("#test").text());//会改变p标签的文本内容
    //Text: 改变了段落内容
});
```

> html() 设置或返回所选元素的内容（包括 HTML 标记）

```js
<p id="test">这是段落中的<b>粗体</b> 文本。</p>

$("#btn1").click(function(){
    alert("HTML: " + $("#test").html());//并不只是获取p标签的内容，还包含其中的标签。
    //HTML: 这是段落中的 <b>粗体</b> 文本。
});

//-----------设置节点内容---------------
$("#btn1").click(function(){
    $("#test").html('改变了<b>段落</b>内容')//改变了p标签的内容包括其中的节点
    alert("HTML: " + $("#test").html();//
    //HTML: 改变了<b>段落</b>内容
});
```

> val() 设置或返回表单字段的值

```js
<input type="text" id="test" value="我是abc">

$("button").click(function(){
    alert("值为: " + $("#test").val());//获取的是表单元素的value值
    //我是abc
});

//------------设置value值------------------
$("button").click(function(){
    $("#test").val('改变value值')//改变value值
    alert("值为: " + $("#test").val());//获取的是表单元素的value值
    //改变value值
});
```

## DOM操作--获取属性
* attr('属性名')---用于获取属性值。

```js
<a href="http://www.baidu.com" id="abc">百度</a>

$("button").click(function(){
    alert($("#runoob").attr("id"));//用于获取属性id的值，
    //abc
})
```

* attr('属性名'，'属性值')---用于设置属性值。
    * 设置一个属性值

    ```js
    <a href="http://www.baidu.com" id="abc">百度</a>
    
    $("button").click(function(){
        alert($("#runoob").attr("id",'address'));//用于设置一个属性id的值，
        //abc
    })
    ```
    * 设置多个属性值
    
    ```js
    <a href="http://www.baidu.com" id="abc">百度</a>
    
    $("button").click(function(){
        alert($("#runoob").attr({
            "id",'address',
            'href','www.baidu2.com'
        });//用于设置多个属性id的值和href的值，
        //abc
    })
    ```

* attr('属性名',callback)---可以带回调函数
    回调函数提供两个参数，第一个是选中元素的下标。第二个是对应属性的value值。
    return 后的值是新的属性值

    ```js
    <a href="http://www.baidu.com" id="abc">百度</a>
    
    $("button").click(function(){
        $("#runoob").attr("id",function(i,oriValue){
            alert(i);
            alert(oriValue);
            return oriValue + 'd';//id:abcd
        });
        //abc
    })
    ```
## 添加元素
* append() - 在被选元素内的结尾插入内容（元素内部）
* prepend() - 在被选元素内的开头插入内容（元素内部）
* after() - 在被选元素之后插入内容（元素外）
* before() - 在被选元素之前插入内容（元素外）

> append() - 在被选元素内的结尾插入内容
> prepend() - 在被选元素内的开头插入内容

* 添加一个元素或文本

    ```js
    $("#btn1").click(function(){
        $("p").append(" <b>追加文本</b>。");//在p标签内部的结尾处添加b标签
    });
    ```
* 添加多个元素或文本

    ```js
    function appendText(){
        var txt1="<p>文本。</p>";              // 使用 HTML 标签创建文本
        
        var txt2=$("<p></p>").text("文本。");  // 使用 jQuery 创建文本
        
        var txt3=document.createElement("p");
        txt3.innerHTML="文本。";           // 使用 DOM 创建文本 text with DOM
        
        $("body").append(txt1,txt2,txt3);        // 追加新元素
    }

    $('button').click(function(){//点击添加事件
        appendText();//会一下在body中的末尾位置添加三个p标签并添加文本。
    })
    ```
    
        prepend()方法与append类似

> after() - 在被选元素之后插入内容（元素外）
> before() - 在被选元素之前插入内容（元素外）

* 添加一个元素或文本

    ```js
    $("#btn1").click(function(){
        $("img").before("<b>之前</b>");//会在img标签的外面之前添加b标签
    });
    ```
* 添加多个元素或文本

    ```js
    function afterText(){
        var txt1="<b>I </b>";                    // 使用 HTML 创建元素
        
        var txt2=$("<i></i>").text("love ");     // 使用 jQuery 创建元素
        
        var txt3=document.createElement("big");  // 使用 DOM 创建元素
        txt3.innerHTML="jQuery!";
        
        $("img").after(txt1,txt2,txt3);          // 在图片后添加文本
    }
    
    $('button').click(function(){//点击添加事件
        afterText();//会一下在img元素标签外之后位置添加三个标签并添加文本。
    })
    ```
    
        before()方法与after()方法类似

## 删除元素
* remove() - 删除被选元素（及其子元素）
* empty() - 从被选元素中删除子元素

```html
<div id="div1" style="height:100px;width:300px;border:1px solid black;background-color:yellow;">
    
    这是 div 中的一些文本。
    <p>这是在 div 中的一个段落。</p>
    <p class="italic">这是在 div 中的另外一个段落。</p>

</div>
```

> remove() - 删除被选元素（及其子元素）

```js
$("#div1").remove();//该方法会删除div元素及其中的文本和子元素。
```


> empty() - 从被选元素中删除子元素
```js
$("#div1").empty();//该方法会删除div元素中的所有子元素及其文本
```

    remove()方法接受一个参数，用来过滤被删除的元素

```js
$("p").remove(".italic");//该方法会删除class="italic"的p元素。并不会删除其他的P元素。
```

##  获取并设置 CSS 类

* addClass() - 向被选元素添加一个或多个类
* removeClass() - 从被选元素删除一个或多个类
* toggleClass() - 对被选元素进行添加/删除类的切换操作
* css() - 设置或返回样式属性

```css
.blue { color:blue; }
.fontW { font-weight:bold; }
```

> **addClass()** - 向被选元素添加一个或多个类
    
* 给多个元素添加一个类

    ```js
    $("button").click(function(){
        $("h1,h2,p").addClass("blue");//给多个元素添加相同的类名blue;
        $("div").addClass("fontW");
    });
    ```
* 给多个元素添加多个类

    ```js
    $("button").click(function(){
        $("h1,h2,p").addClass("blue fontW");//给多个元素添加相同的多个类名blue fontW
    });
    ```

> **removeClass()** - 从被选元素删除一个或多个类

* 从元素中删除一个类

    ```js
    $("button").click(function(){
        $("h1,h2,p").removeClass("blue");//从元素中删除一个类名blue；
    });
    ```
* 从元素中删除多个类名

    ```js
    $("button").click(function(){
        $("h1,h2,p").removeClass("blue fontW");//从元素中删除多个类名blue和fontW；
    });
    ```

> **toggleClass()** - 对被选元素进行添加/删除类的切换操作

```js
$("button").click(function(){
    $("h1,h2,p").toggleClass("blue");//标签中内容的颜色在按钮点击后会在黑色和蓝色之间切换
});
```

> **css()** - 设置或返回样式属性

    只有属性时，作用是返回该属性的属性值。

* 返回指定的css属性的值

    ```js
    $("p").css("background-color");//返回p标签的背景色；返回的是rgba色。
    ```
* 设置一个指定的css属性的值
    *第一个参数是需要设置的属性，第二个参数为新的属性值。*

    ```js
    $("p").css("background-color","yellow");//将所有p标签的背景色设为yellow；
    ```
* 设置多个指定的css属性的值
        
    *把需要设置的属性和属性值用key+value的形式写在* 对象 *中传入css中。*

    ```js
    $("button").click(function(){
        $("p").css( {"background-color":"yellow","font-size":"200%"} );
    });
    ```
## 宽度和高度（尺寸）

* width()--元素本身的宽度
* height()--元素本身的高度
* innerWidth()--元素本身宽度+padding
* innerHeight()--元素本身的高度+padding
* outerWidth()--元素本身的宽度+padding+border
* outerHeight()--元素本身的告诉+padding+border

> **width()**获取元素本身的宽度
> **width('400px')**设置元素的宽度为400px;

```js
$("button").click(function(){
    alert( $("#div").width(); )//获取div的宽度
    $("#div").width('500px');//设置div的宽度为500px;
});
```
*innerWidth(),outerWidth()方法与之类似,也可以设置相应的值，但是设置的值都是元素本身的宽高*

# 遍历

## 向上遍历祖先节点

* parent() --- 找到指定元素的直接父元素
* parents() --- 找到指定元素的所有祖先元素
* parentsUntil('界定元素') --- 找到指定元素与界定元素之间的所有祖先元素 

> parent() --- 找到指定元素的直接父元素

> 语法： $(selector).parent();

```js
$("span").parent();//找到所有span标签的直接的fu'yuan
```

> parents() --- 找到指定元素的所有父元素

```js
$("span").parents();//找到所有span标签的所有父元素，直到<html>标签为止(包括html)
```

- 对parents('ul')进行过滤

```js
$("span").parents('p');//找到所有span标签的所有父元素中标签为p的所有标签，直到<html>标签为止(包括html)
```

> parentsUntil('界定元素') --- 找到指定元素与界定元素之间的所有祖先元素 

```js
$("span").parentsUntil("div");//找到span标签和div标签中所有的span祖先节点，如果有多个div，则找最近的div，在之间找。
```

## 向下遍历子元素

* children() --- 返回被选元素的所有直接子元素。
* find('*') --- 返回被选元素的后代元素，一路向下直到最后一个后代。

> children() --- 返回被选元素的所有直接子元素。

```js
$("div").children();//找到div元素下的所有的直接子元素。
```

* 对获取到的子元素进行过滤children('p')

```js
$("div").children('p');//找到div元素下的所有的直接子元素中标签为p的元素。
```

> find('*') --- 返回被选元素的所有的后代元素，一路向下直到最后一个后代。

```js
$("div").find("*");//找到div标签下所有的子代元素；
```

```js
$("div").find("p");//找到div标签下所有的标签为<p>的子代元素；
```

## 同胞遍历

* siblings() --- 找到指定元素父级下的所有的兄弟元素，不包括自己
* next() --- 找到指定元素的下一个兄弟元素（一个）
* nextAll() --- 找到指定元素之后的所有兄弟元素（大于等于一个）
* nextUntil('限定元素') --- 找到指定元素之后和限定元素间的所有兄弟元素
* prev() --- 找到指定元素的上一个兄弟元素（一个）
* prevAll() --- 找到指定元素之前的所有兄弟元素（大于等于一个）
* prevUntil('限定元素') --- 找到指定元素之前和限定元素间的所有兄弟元素

> siblings() --- 找到指定元素父级下的所有的兄弟元素

```js
$("h2").siblings();//找到h2标签的所有兄弟节点
```
**过滤全部兄弟节点**
```js
$("h2").siblings("p");//找到h2兄弟节点中标签为<p>的节点
```

> next() --- 找到指定元素的下一个兄弟元素（一个）

```js
$("h2").next();//找到h2标签的下一个兄弟节点
```

> nextAll() --- 找到指定元素之后的所有兄弟元素(>=1)

```js
$("h2").nextAll();//找到h2标签之后的所有兄弟节点；
```
**过滤之后兄弟节点**
```js
$("h2").nextAll('p');//找到h2标签之后的所有兄弟节点中标签为<p>的节点；
```

> nextUntil('限定元素') --- 找到指定元素之后和限定元素间的所有兄弟元素

```js
$("h2").nextUntil("h6");//找到h2标签和h6标签中所有的兄弟节点，不包含他俩。
```

## 遍历-过滤方法

* first() --- 被选元素的首个元素
* last() --- 被选元素的最后一个元素。
* eq() --- 返回被选元素中带有指定索引号的元素。
* filter() --- 返回匹配的元素
* not() --- 返回不匹配标准的所有元素。

> first() --- 被选元素的首个元素

```js
$("div p").first();//找到第一个div中的第一个p标签；并不是所有div下的第一个P标签。
```
> last() --- 被选元素的最后一个元素。

```js
$("div p").last();//找到最后一个div中的最后一个p标签；并不是所有div下的最后一个P标签。
```
> eq() ---  返回被选元素中带有指定索引号的元素。正数从前往后找，负数从后往前找
```html
<p>菜鸟教程 (index 0).</p>
<p>http://www.runoob.com (index 1)。</p>
<p>google (index 2).</p>
<p>http://www.google.com (index 3)。</p>

```
```js
 $("p").eq(1).css("background-color","yellow");
```










