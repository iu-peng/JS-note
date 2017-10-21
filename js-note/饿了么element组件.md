# 饿了么element组件



---

## elementUI语法
### 分栏宽度 --：span="n" ---总共24栏
```html
<el-row>
    <el-col :span="6">  </el-col>
</el-row>
```
### 每一行中分栏之间的间隔 --：gutter="n"---定义的是像素

```html
<el-row  :gutter="20">
    <el-col :span="6">  </el-col>
</el-row>
```
### 定义分栏距左边的栏数
```html
<el-row  :gutter="20">
    <el-col :span="6"  :offset="6">  </el-col>
</el-row>
```
offset指的是距离左边最近的分栏的分栏数（总共24栏）
```html
<el-row  :gutter="20">
    <el-col :span="6"   :push="1">  </el-col>
</el-row>
```
：push="1" -- 定义栅格向右移动的格数
：pull="1" -- 定义栅格向左移动的格数

### 通过flex定义分栏的对齐方式
需要在<el-row>标签中定义属性 type="flex"

```html
<el-row type="flex" class="row-bg" justify="center">
    <el-col :span="6"><div class="grid-content bg-purple"></div></el-col>
    <el-col :span="6"><div class="grid-content bg-purple-light"></div></el-col>
    <el-col :span="6"><div class="grid-content bg-purple"></div></el-col>
</el-row>
```
justify="center"：全部挤在中间
justify="end" ： 全部挤在右边
justify="space-between" ： 
justify="space-around" ： 分散居中对其

### 响应式布局
```html
<el-row :gutter="10">
  <el-col :xs="8" :sm="6" :md="4" :lg="3"><div></div></el-col>
  <el-col :xs="4" :sm="6" :md="8" :lg="9"><div></div></el-col>
  <el-col :xs="4" :sm="6" :md="8" :lg="9"><div></div></el-col>
  <el-col :xs="8" :sm="6" :md="4" :lg="3"><div></div></el-col>
</el-row>
```
在不同分辨率下展示不同的宽度，在最小屏 的时候这4列是8448的布局，再大点是6666的布局，中等屏幕是4884的布局，大屏是3993的布局

### 引入图标
```html
<i class="el-icon-delete"></i>
```
以上引入了一个删除图标
```html
<el-button type="primary" icon="search">搜索</el-button>
定义一个搜索框
```
### 添加按钮

|属性类型|size|type|plain|loading| disabled|icon|autofocus|native|
|:------:|  :------:  | :------:|:------:|:------:|:------:|
|意义|尺寸|类型|是否朴素按钮|是否加载中|是否禁用|定义图标|是否默认聚焦|原生type属性|


正常按钮
```html
<el-button>默认按钮</el-button>  //默认白底按钮
<el-button type="primary">主要按钮</el-button>//蓝底按钮
<el-button type="text">文字按钮</el-button>//无边框文字按钮
```
不可点击按钮
```html
<el-button :plain="true" :disabled="true">默认按钮</el-button>
需要添加属性：plain="true" 否则和主要按钮的状态类似
<el-button type="primary" :disabled="true">主要按钮</el-button>
灰底灰字
<el-button type="text" :disabled="true">文字按钮</el-button>
灰色字
```
* 带颜色倾向的
    默认颜色

    ```html
    <el-button type="success">success</el-button>
    //绿色
    <el-button type="warning">warning</el-button>
    //橘色
    <el-button type="danger">danger</el-button>
    //红色
    <el-button type="info">info</el-button>
    //蓝色
    ```
    hover时的按钮 -- 没有hover时默认`<el-button>默认按钮</el-button>`这样的白底黑字按钮
    hover时会成为边框和字体成为默认颜色的按钮，背景色都是白色
    ```html
    <el-button :plain="true" type="success">success</el-button>
    <el-button :plain="true" type="warning">warning</el-button>
    <el-button :plain="true" type="danger">danger</el-button>
    <el-button :plain="true" type="info">info</el-button>
    ```
图标按钮 -- 只需要在标签内部定义属性icon=""即可，也可以更换按钮的type样式
```html
<el-button type="primary" icon="edit">编辑</el-button>
<el-button type="primary" icon="share">分享</el-button>
<el-button type="primary" icon="delete">删除</el-button>
<el-button type="primary" icon="search">搜索</el-button>
<!-- 默认字在图标的右侧 -->
<el-button type="primary">搜索<i class="el-icon-search el-icon--right"></i></el-button>
<!-- 在字的后面添加一个标签定义图标即可 -->
```
按钮组
```html
<el-button-group>
  <el-button type="primary" icon="arrow-left">上一页</el-button>
  上一页：有个向左的箭头
  <el-button type="primary">下一页<i class="el-icon-arrow-right el-icon--right"></i></el-button>
  下一页：有个向右的箭头 在文字后面定义一个class中有图标的即可，--right是靠右
</el-button-group>
```
显示加载按钮
```html
<el-button type="primary" :loading="onoff" @click="onoff =!onoff">点击后变加载</el-button>
```
设置按钮尺寸 --- 通过设置size即可  large small mini
```html
<el-button size="large">大图标</el-button>
<el-button>正常图标</el-button>
<el-button size="small">小图标</el-button>
<el-button size="mini">mini图标</el-button>
```
### 单选按钮

|    +|单选属性|单选按钮组属性|单选按钮组事件|
|:------:|  :------:  | :------:|:------:|:------:|:------:|
|属性名|**label**|**size**|**change**|
|解释|Radio 的 value|Radio 按钮组尺寸|绑定值变化时触发的事件|
|属性名|**disabled**|**fill**|
|解释|是否禁用|按钮激活时的填充色和边框色|
|属性名|**name**|**text-color**|
|解释|原生 name 属性|按钮激活时的文本颜色|

> 选中的条件是：单选的label值和绑定的组件data中的值相等
> 如果刚进入时不相等，则默认不相等，点击后绑定的data中相应的值会和单选的label相等
> 选中的条件是绑定的变量值等于label中的值

基础单选 --- 绑定`v-model` 设置`label`
```html
<el-radio class="radio" v-model="radio" label="100">{{radio}}</el-radio>
<el-radio class="radio" v-model="radio" label="222">{{radio}}</el-radio>
```
设置单选禁用状态 --- `disabled`
```html
<el-radio disabled class="radio" v-model="radio" label="100">{{radio}}</el-radio>
```
单选按钮组 --- `el-radio-group`  绑定`v-model`
按钮组中的每个单选按钮如果label前设置了冒号:，则会一刷新就判断是否应该选中，不加不会主动选中
```html
<el-radio-group v-model="radio">
  <el-radio :label="1"></el-radio>
  <el-radio :label="2"></el-radio>
  <el-radio :label="3"></el-radio>
</el-radio-group>
```
单选按钮样式 --- 按钮式的单选`el-radio-button`
```html
<el-radio-group v-model="radio">
  <el-radio-button :label="1"></el-radio-button>
  <el-radio-button :label="2"></el-radio-button>
  <el-radio-button :label="3"></el-radio-button>
</el-radio-group>
```

### 多选按钮
基础多选 --- 多选绑定的data中的值和label相等时，默认选中    
```html
<el-checkbox v-model="checbox" label="a"></el-checkbox>
```
设置禁用状态 --- `disabled`
```html
<el-checkbox v-model="checked1" disabled>备选项1</el-checkbox>
```
多选框按钮组 --- 多选不需要在label前添加冒号：，否则报错
v-model绑定的可以是data中的数组
```html
<el-checkbox-group v-model="checbox">
  <el-checkbox label="a"></el-checkbox>
  <el-checkbox label="b"></el-checkbox>
  <el-checkbox label="c"></el-checkbox>
</el-checkbox-group>
```
多选个数限制
```html
<el-checkbox-group v-model="checkedCity" :min="1" :max="2">
  <el-checkbox v-for="city in cities" :label="city"></el-checkbox>
</el-checkbox-group>

```














