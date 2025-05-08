# Vue框架 2.x

- Vue是一套前端框架，免除原生JavaScript中的DOM操作，简化书写。

- 基于MVVM(Model-View-ViewMode)思想，实现数据的**双向绑定**，将编程的关注点放在数据上。

​	https://v2.cn.vuejs.org/

### 快速入门

1. 引入：`<script src="js/vue.js"></script> `（head中）
2. 创建Vue核心对象，定义数据类型 （script标签中）

```javascript
<script>
    new Vue({
    	el:"#app",	// 控制区域
    	data: {
            message:"Hello Vue!"
        }
	})
</script>
```



3. 编写视图 （body中）

```javascript
<div id="app">
    <input type="text" v-model="message">
    {{ message }}	// 插值表达式
</div>
```

### 常用指令

- v-bind	为HTML标签绑定属性值，如 设置href, css样式等
- v-model       在表单元素上创建双向数据绑定
- v-on        为HTML标签绑定事件
- v-if    v-else-if    v-else        条件性的渲染某元素
- v-show        根据条件展示某元素，区别在于切换的是display属性的值
- v-for         列表渲染，遍历容器的元素或者对象的属性

#### 生命周期

### API风格

- 组合式API（推荐）：<script setup></script>
  - const varName = ref(0) 声明响应式变量；function声明函数；onMounted( ()=>{声明钩子函数} )
- 选项式API：
  - data()声明响应式对象；methods: 声明方法；mounted()声明钩子函数