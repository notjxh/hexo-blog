---
title: vue-start
date: 2023-09-15 10:38:49
tags: vue
description:
categories: 前端
---

### 第一章、Vue基础语法
> 学习前需要掌握html/css/javascript，包括es6语法
#### 1. 第一个Vue实例

在head中引入vue.js  
```
 <script src="vue.js"></script>
```
定义vue实例，`el`绑定挂载点，`data`定义数据，通过插值表达式`{{..}}`取值  
```
<body>

<h1 id="root">{{message}}</h1>

<script>
    let app = new Vue({
        el: '#root',
        data: {
            message: 'hello vue'
        }
    });
</script>
</body>
```
#### 2. 挂载点、模板、实例

挂载点：`el`绑定的dom节点  
模板：挂载点下的所有内容，也可以写在实例里的template里（这种情况下挂载点下其他节点无效）  

```
    let app = new Vue({
        el: '#root',
        template:'<h1>this is template {{message}}</h1>',
        data: {
            message: 'hello vue'
        }
    });
```
实例：new Vue()会结合模板和数据展示在挂载点上

#### 3. vue实例中的数据、事件和方法

数据：

1）通过插值表达式`{{}}`获取数据  
2）通过模板指令v-text/v-html获取数据  
3）v-html不会转义html标签  

事件和方法：

通过v-on:click="functionName"绑定事件  
将方法写在vue实例的methods里  
v-on:可以简写成@

```
<div id="root">
<!--    <div v-text="content"></div>-->
    <div v-on:click="handleClick">{{msg}}</div>

</div>
<script>
    let app = new Vue({
        el: '#root',
        // template:'<h1>this is template {{message}}</h1>',
        data: {
            msg: 'hello',
            content: '<h1>content</h1>'
        },
        methods:{
            handleClick:function () {
                this.msg='world';
            }
        }
    });
</script>
```
#### 4. vue里的属性绑定和双向数据绑定
v-bind:属性名=数据名 绑定  可以简写为:属性名=数据名  
双向绑定：即改变当前绑定的属性时，绑定数据也会发生改变  
v-model:属性名=数据名（v-model=数据名）

```
<div id="root">
    <div v-bind:title='title'>数据绑定</div>
    <div :title='title'>数据绑定简写</div>
    <input v-model= 'title'/>
    <input v-model:value= 'title'/>
    <div>{{title}}</div>
</div>
```



#### 5. vue中的计算属性和侦听器

computed：定义一个计算属性  
watch：对数据/计算属性进行侦听，当发生改变时，做一些操作

```
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>vue中的计算属性和侦听器</title>
    <script src="vue.js"></script>
</head>
<body>
<div id="root">
名：<input v-model="firstName">
姓：<input v-model="lastName">
    全名：<div>{{fullName}}</div>
    count:<div>{{count}}</div>
</div>
<script>
    new Vue({
        el:'#root',
        data:{
            firstName:'',
            lastName:'',
            count:0
        },
        computed:{
            fullName:function () {
                return this.firstName+this.lastName;
            }
        },
        watch:{
            firstName:function(){
                this.count++
            },
            lastName:function(){
                this.count++
            }
        }

    })
</script>
</body>
</html>
```


#### 6. 模板指令v-if、v-show、v-for

> 需求:通过toggle按钮实现div的显示隐藏

通过v-if='true/false'来控制时，会直接当前节点隐藏，  
通过v-show='true/false'，只是将display=none  

将list中的每一个元素item放在`<li>`标签中，增加:key=''能增加效率，但是key不能重复
```
<li v-for="(item,index) of list" :key="index">{{item}}</li>
```



```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title> 模板指令v-if、v-show、v-for</title>
    <script src="vue.js"></script>
</head>
<body>
<div id="root">
    <div v-show="show">{{msg}}</div>
    <input type="button" @click="handleClick" value="toggle"/>
    <div>
        <ul>
            <li v-for="(item,index) of list" :key="index">{{item}}</li>
        </ul>
    </div>

</div>
<script>
    new Vue({
        el: '#root',
        data: {
            msg: '显示',
            show: true,
            list: [4, 4, 5]
        },
        methods: {
            handleClick: function () {
                this.show = !this.show;
            }
        }

    })
</script>
</body>
</html>
```


### 第二章、vue中的组件
#### 7. 实现一个简单的todo-list功能

```
<body>
<div id="root">
    <div>
    <input v-model="inputValue"/>
    <button @click="handleSubmit">提交</button>
    </div>
    <ul>
        <li v-for="(item,index) of list">{{item}}</li>
    </ul>
</div>
<script>
    new Vue({
        el:'#root',
        data:{
            inputValue:'',
            list:[1,2,3,4]
        },
        methods:{
            handleSubmit:function () {
                this.list.push(this.inputValue);
                this.inputValue='';
            }
        }

    })
</script>
</body>
```


#### 8. 实现一个简单的todo-list-组件版
1）将list拆分成一个组件
方式一：定义一个局部组件，通过vue实例中的componens引用

```
    let todo_item={
        template: '<li>item</li>'
    };
```

```
        components:{
            todo_item:todo_item
        },
```
方式二：通过Vue.componet定义一个全局组件,props用来接收外部组件的传参

```
    Vue.component('todo_item',{
        props:['content'],
        template:`<li>{{content}}</li>`
    });
```
2）在模板中引用组件，并且通过**属性content传参**

> :content="item"  好像只能用:content传参？

```
    <div id="root">
        <div>
        <input v-model="inputValue"/>
        <button @click="handleSubmit">提交</button>
        </div>
        <ul>
            <todo_item v-for="(item,index) of list" :key="index" :content="item">{{index}}</todo_item>
        </ul>
    </div>
```

3) 组件本身也是个Vue实例

#### 9. 实现todolist的删除功能(父子组件之间的事件交互)

> 需求：点击list中的值时将其删除；采用发布订阅模式

1）在子组件中定义onclick事件：通过`this.$emit(事件名,参数);`告知父组件触发删除事件

```
    Vue.component('todo_item',{
        props:['content','index'],
        template:`<li @click="handleClick">{{content}}</li>`,
        methods:{
            handleClick:function () {
                this.$emit('delete',this.index);
            }
        }
    });
```
2）父组件监听子组件的事件，接收一个参数index

```
    <ul>
        <todo_item v-for="(item,index) of list"
                   :key="index"
                   :content="item"
                   :index="index" 
                   @delete="handleDelete">{{index}}</todo_item>
    </ul>
```

```
    handleDelete:function (index) {
        this.list.splice(index,1);
    }
```

### 第三章、vue-cli的使用
todo 需要时再看吧

#### 10.vue-cli的简介与使用

> vue-cli是vue用来搭建基于webpack的web项目的脚手架工具

> 样式里的scoped表示是局部样式，只对当前组件有效

#### 11.vue-cli改造todo-list项目