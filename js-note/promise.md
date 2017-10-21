# promise


---

## promise

* 通过new Promise()实例一个对象

```js
let pro = new Promise( (resolve,reject)=>{
    //resolve('这是resolve成功')
    reject('这是reject失败')
} )
```
> 接受一个函数作为参数，该函数有两个参数，第一个是代表成功的resolve，第二个是代表失败的reject，在函数中定义需要执行成功还是失败的函数，函数每个参数在函数中都是可以执行的，并可以传参

* promise实例对象的then

> 每个实例后的promise都接受一个then方法，接受两个函数参数，第一个代表成功后的执行的函数，第二个代表失败后执行的函数。

```js
pro.then(
    (data)=>{
        console.log(data)//这是resolve成功
    },
    (data)=>{
        console.log(data)//这是reject失败
    }
)
```
then中的函数参数：接受来自实例resolve或reject的参数，如resolve（'哈哈'），传的参数是哈哈，那么在then的第一个函数参数中，(data)=>{},data就接受这个参数


