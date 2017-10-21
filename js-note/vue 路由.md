# vue 路由



---

路由：分前端路由和后端路由

## 单页应用

> 一个网站只有一个页面，通过一些路由库来配置路径对应的组件，当访问到这个路径的时候，就会渲染对应的组件。

## 路由

> 接收请求分发一个内容

* 使用步骤
    1. 下载组件 `npm i vue-router --save`
    1. 引入vue-router
        `import vueRouter from 'vue-router'`
    2. 把vue-router作为插件使用
    
        ```Vue.use(VueRouter)```
    3. 开始实例化路由

        ```js
        let router = new Router({
        	mode:'history',
        	routes: [
        		{path: '/',component: home},//默认主页
        		{path: '/project',component: project},
        		{path: '/doc',component: doc},
        		{path: '/login',component: login}
        	]
        })
        ```
    4. 把实例化的路由注入到根实例中
    
        ```js
        new Vue({
        	el:'#app',
        	router:router,//在main.js中注册
        	template:'<App/>',
        	components:{
        		App
        	}
        })
        ```
    5. 在app.vue中写上一对标签，是vue-router提供的，`<router-view></router-view>`,用来告诉vue，路径匹配到的组件渲染在这个位置，路径匹配到的组件就会替换router-view这一对标签
    
        ```js
        <template>
        	<div id="app">
        		<router-view></router-view>
        	</div>
        </template>
        ```

## router-link
> <router-link> 组件支持用户在具有路由功能的应用中（点击）导航。 通过 to 属性指定目标地址，默认渲染成带有正确链接的`<a>`标签，可以通过配置 tag 属性生成别的标签.。另外，当目标路由成功激活时，链接元素自动设置一个表示激活的 CSS 类名router-link-active。

> 在一个router-link被选中的时候会增加一个class属性值router-link-active

```js
<router-link to="home" tag="li"></router-link>
//渲染成li标签
```

## 重定向

> 路径不匹配，用`path:'*'`,表示

```js
let router = new VueRouter({
	mode:'history',
	routes:[
		{path:'/',component:home,name:'home',alias:'/haha'},
		{path:'/doc',component:doc},
		{path:'/project',component:project},
		{path:'/login',component:login},
		
		{path:'*',component:notfound},//定向到404页面
		
		//重定向到指定路径
		{path:'*',redirect:'/'}
		//如果路径匹配不到直接重定向到首页
		
		//重定向到指定name的路径
		{path:'*',redirect:{name:'home'}},
		//如果路径匹配不到直接重定向到首页--路径可以定一个name，从而调用
        
		//条件重定向
		//传入一个函数，函数参数是一个，包括目标路由的信息。条件匹配通过return转到另一页面
		{path:'*',redirect:(to)=>{
			if(to.path === '/abc'){
				return '/'//到主页
			}else if(to.path === 'ab'){
				return {name:'home'}//也可以通过name属性跳转
			}else{
				return '/haha'
			}
		}}
	]
})
```
> 路径可以指定name名，方便下面使用，还可以指定别名alias。访问path路径可访问alias路径是一样的

```js
{path:'/',component:home,name:'home',alias:'/haha'}
```



