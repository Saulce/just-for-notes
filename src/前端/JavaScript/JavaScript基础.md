# JavaScript

## 对象

#### Array

定义：

1. let arr = new Array(1, 2, 3, 4);

2. let arr = [1, 2, 3, 4];

属性：

1. length

方法：

1. forEach()
2. push()
3. splice()

#### String

定义：

1. let str = new String("Hello String");

2. let str = "Hello String";

3. let str = 'Hello String';

属性：

1. length

方法：

1. charAt()
2. indexOf()
3. trim()
4. substring()

#### JSON (JavaScript Object Notation)

自定义对象定义：let 对象名{

​					属性名1：属性值1，

​					函数名称：function(形参列表){}

​				};

JSON是通过JavaScript对象标记法书写的文本。——JSON格式化校验工具

其语法简单，层次结构鲜明，现多作为**数据载体**使用

基础语法：

1. 定义：let 变量名 = '{"key1" : value1, "key2" : value2}';	value数据类型为：数字、"字符串"、true或false、[数组]、{对象}、null
2. JSON字符串转化为JS对象：let jsObject = JSON.parse(jsonStr);
3. JS对象转化为JSON字符串：let jsonStr = JSON.stringify(jsObject);

### BOM

- Window  浏览器窗口对象
- Navigator  浏览器对象
- Screen  屏幕对象
- History  历史记录对象
- Location  地址栏对象

详情见 <font color='blue'>w3school</font>

### DOM

- Document  整个文档对象
- Element  元素对象
- Attribute  属性对象
- Text  文本对象
- Comment  注释对象

详情见 <font color='blue'>w3school</font>

## 事件监听

#### 事件绑定

- 方法一：通过HTML标签中的事件属性进行绑定
- 方法二：通过DOM元素属性绑定

#### 事件类型

addEventListener(事件，事件处理函数)

鼠标事件

- click  鼠标点击
- mouseenter  鼠标经过
- mouseleave  鼠标离开

焦点事件

- focus  获得焦点
- blur  失去焦点

键盘事件

- keydown  键盘按下触发
- keyup  键盘抬起触发

文本事件

- input  用户输入事件

## Web APIs

### DOM

#### 获取DOM元素

根据CSS选择器获取

#### 操作元素内容和属性

#### 修改样式

1. 通过style
2. 通过类名
3. 通过classList

————————————————————————————————————————————————————————————

### 案例

#### 轮播图

#### 搜索框

#### 发布评论

#### 页面侧边栏和返回顶部

#### 导航滑动

### BOM

————————————————————————————————————————————————————————————

### 案例

#### 验证码

#### 验证用户名

#### 放大镜

### 正则表达式

​	正则表达式是一组由字母和符号组成的特殊文本，它可以用来从文本中找出满足你想要的格式的句子。通俗的讲就是**按照某种规则去匹配符合条件的字符串。**（各种语言通用）

- 定义语法规则
  - const 变量名 = /表达式/

- 匹配
  - test() 方法判断是否符合规则——返回boolean类型（重点）
  - exec() 方法查找符合规则的字符串——返回字符数组

#### 元字符

##### 边界符

##### 量词

##### 字符类

#### 修饰符

- 替换	字符串.replace(/正则表达式/, '替换的文本')