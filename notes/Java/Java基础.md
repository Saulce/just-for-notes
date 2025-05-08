# Java

强大的生态！

### 注释

1. 单行注释：//
2. 多行注释（不常用）
3. 文档注释：/** */

代码的注释不是越详细越好。实际上好的代码本身就是注释，我们要尽量规范和美化自己的代码来减少不必要的注释。

若编程语言足够有表达力，就不需要注释，尽量通过代码来阐述。

### 运算符

#### 	隐式转换

自动类型提升		'123' + 123 = '123123'

#### 	强制转换

int = (int)

####			逻辑运算符

`& 与` `| 或` `^ 异或` `! 非` `&& 短路与` `|| 短路或`（短路效率更高，因为当左边的表达式能确定最后的结果，右边的就不会执行了）	

#### 移位运算符

在 Java 代码里使用` << 左移`、`>> 带符号右移`和`>>> 无符号右移`转换成的指令码运行起来会更高效些。掌握最基本的移位运算符知识还是很有必要的，这不光可以帮助我们在代码中使用，还可以帮助我们理解源码中涉及到移位运算符的代码。

由于 `double`，`float` 在二进制中的表现比较特殊，因此不能来进行移位操作。移位操作符实际上支持的类型只有`int`和`long`，编译器在对`short`、`byte`、`char`类型进行移位前，都会将其转换为`int`类型再操作。

### 数据类型

- boolean/1
- byte/8
- char/16
- short/16
- int/32
- float/32
- long/64
- double/64

这八种基本类型都有对应的包装类分别为：`Byte`、`Short`、`Integer`、`Long`、`Float`、`Double`、`Character`、`Boolean` ，基本类型与其对应的包装类型之间的赋值使用自动装箱与拆箱完成。

**注意：**

1. Java 里使用 `long` 类型的数据一定要在数值后面加上 **L**，否则将作为整型解析。
2. `char a = 'h'`char :单引号，`String a = "hello"` :双引号。

#### 基本类型和包装类型的区别

**用途**：除了定义一些常量和局部变量之外，我们在其他地方比如方法参数、对象属性中很少会使用基本类型来定义变量。并且，包装类型可用于泛型，而基本类型不可以。

**存储方式**：基本数据类型的局部变量存放在 Java 虚拟机栈中的局部变量表中，基本数据类型的成员变量（未被 `static` 修饰 ）存放在 Java 虚拟机的堆中。包装类型属于对象类型，我们知道几乎所有对象实例都存在于堆中。

**占用空间**：相比于包装类型（对象类型）， 基本数据类型占用的空间往往非常小。

**默认值**：成员变量包装类型不赋值就是 `null` ，而基本类型有默认值且不是 `null`。

**比较方式**：对于基本数据类型来说，`==` 比较的是值。对于包装数据类型来说，`==` 比较的是对象的内存地址。**所有整型包装类对象之间值的比较，全部使用 `equals()` 方法。**

⚠️ 注意：**基本数据类型存放在栈中是一个常见的误区！** 基本数据类型的存储位置取决于它们的作用域和声明方式。如果它们是局部变量，那么它们会存放在栈中；如果它们是成员变量，那么它们会存放在堆中。

**为什么说是几乎所有对象实例都存在于堆中呢？** 

这是因为 HotSpot 虚拟机引入了 JIT 优化之后，会对对象进行***逃逸分析***，如果发现某一个对象并没有逃逸到方法外部，那么就可能通过标量替换来实现栈上分配，而避免堆上分配内存。

#### 缓存机制 缓存池

Java 基本数据类型的包装类型的大部分都用到了缓存机制来提升性能。

`Integer i1=40` 这一行代码会发生装箱，也就是说这行代码等价于 `Integer i1=Integer.valueOf(40)` 。

编译器会**在缓冲池范围内的基本类型**自动装箱过程调用 valueOf() 方法，因此多个 Integer 实例使用自动装箱来创建并且值相同，那么就会引用相同的对象。

new Integer() 与 Integer.valueOf() 的区别在于:

- new Integer() 每次都会新建一个对象
- Integer.valueOf() 会使用缓存池中的对象，多次调用会取得同一个对象的引用。

如果超出对应范围仍然会去创建新的对象，缓存的范围区间的大小只是在性能和资源之间的权衡。

两种浮点数类型的包装类 `Float`,`Double` 并没有实现缓存机制。

#### 自动装箱与自动拆箱

- **装箱**：将基本类型用它们对应的引用类型包装起来；
- **拆箱**：将包装类型转换为基本数据类型；

从字节码中，发现装箱其实就是调用了 包装类的`valueOf()`方法，拆箱其实就是调用了 `xxxValue()`方法。

因此，

- `Integer i = 10` 等价于 `Integer i = Integer.valueOf(10)`
- `int n = i` 等价于 `int n = i.intValue()`;

注意：**如果频繁拆装箱的话，也会严重影响系统的性能。我们应该尽量避免不必要的拆装箱操作。**

### 判断与循环

#### continue、break 和 return 的区别

- `continue`：指跳出当前的这一次*循环*，继续下一次循环。

- `break`：指跳出整个*循环体*，继续执行循环下面的语句。

- `return` 用于跳出所在*方法*，结束该方法的运行。return 一般有两种用法：
  1. `return;`：直接使用 return 结束方法执行，用于没有返回值函数的方法
  2. `return value;`：return 一个特定值，用于有返回值函数的方法

#### case穿透

语句中没有break导致的（如果多个case的语句体重复了，可以考虑用case穿透简化代码）

switch新特性（JDK12）：**case** value -> { }  简化省略了break

**无限循环**：for(;;){}	while(true){}	do{……}while(true);	注：一般用**while()**语句

### 数组

1. 声明与初始化（例如）：(一维)String[] arr1 = new String[5];

​						 (二维)String[] [] arr2 = new String[3] [2];	+	String[] [] arr3 = new String[3] [];

2. 数组元素的调用：通过下标

3. 数组的属性：length  注：数组一旦初始化，其长度就是确定的，不可修改
4. 数组的遍历
5. 数组元素的默认初始化值：整型：0        浮点型：0.0        char型：0或'\u0000'而非'0'        boolean型：false        引用数据类型：null    

数组属于引用数据类型，数组的元素也可以是引用数据类型，即一维数组的元素也可以是一维数组——二维数组。

(二维）外层元素 arr[0]	内层元素 arr[0] [1]	内外层元素的初始值为：（[3] [2]）外为地址值，内与一维初始化情况相同----------------([3] [ ]）外为 null，内不能调用，否则报错

6. 数组的内存解析：堆、元空间、栈、本地方法栈、寄存器

数组常见问题：(1) 数组角标越界异常 （2）空指针异常

### 字符串

String 被声明为 final，因此它不可被继承。内部使用 char 数组存储数据，该数组被声明为 final，这意味着 value 数组初始化之后就不能再引用其它数组。并且 String 内部没有改变 value 数组的方法，因此可以保证 String 不可变。

1. 构造方法：
   1. 直接赋值：String s0 = "abc";
   2. 空参构造：String s1 = new String();
   3. 有参构造（传递字符数组）：`char[] chs = {'a', 'b', 'c'};`        `String s2 = new String(chs);  // 这句话会创建1或2个字符串对象`
      - 修改字符串内容—>String不可变—>修改字符数组以间接修改字符串
   4. 有参构造（传递字节数组），应用场景：网络传输

**字符串常量池：**是 JVM 为了提升性能和减少内存消耗针对字符串（String 类）专门开辟的一块区域，主要目的是为了避免字符串的重复创建。

#### 比较

`String` 中的 `equals` 方法是被重写过的。 `Object` 的 `equals` 方法是比较的对象的内存地址，而 `String` 的 `equals` 方法比较的是字符串的值是否相等。

- boolean equals(要比较的字符串)：完全相同结果为true，否则false
- boolean equalsIgnoreCase(要比较的字符串)：忽略大小写的比较

#### 遍历

- charAt(int index)  根据索引返回字符
- length()  返回字符串长度——是方法，数组的length是属性

#### 拼接和反转

Java 语言本身并不支持运算符重载，“+”和“+=”是专门为 String 类重载过的运算符，也是 Java 中仅有的两个重载过的运算符。

字符串对象通过“+”的字符串拼接方式，实际上是通过 `StringBuilder` 调用 `append()` 方法实现的，拼接完成之后调用 `toString()` 得到一个 `String` 对象 。不过，在循环内使用“+”进行字符串的拼接的话，存在比较明显的缺陷：**编译器不会创建单个 `StringBuilder` 以复用，会导致创建过多的 `StringBuilder` 对象**。

直接使用 `StringBuilder` 对象进行字符串拼接的话，就不会存在这个问题了。

不过，使用 “+” 进行字符串拼接会产生大量的临时对象的问题在 JDK9 中得到了解决。在 JDK9 当中，字符串相加 “+” 改为了用动态方法 `makeConcatWithConstants()` 来实现，而不是大量的 `StringBuilder` 了。这也意味着 JDK 9 之后，可以放心使用“+” 进行字符串拼接了。

#### 屏蔽

- 截取  String substring(int beginIndex, int endIndex)    ps. 包头不包尾，包左不包右；返回值才是截取的结果
- 替换  replace(target, replacement)

#### StringBuilder

可以看成是一个容器，创建后里面的内容是可变的。可以节省性能，提高效率。

构造方法：

- StringBuilder()    创建一个空白可变的字符串对象
- StringBuilder(String str)    根据字符串的内容，创建可变字符串对象

成员方法：

- append()  添加数据并返回对象本身
- reverse()  反转
- length()  返回长度
- toString()  把StringBuilder转换为String

`StringBuilder` 与 `StringBuffer` 都继承自 `AbstractStringBuilder` 类，在 `AbstractStringBuilder` 中也是使用字符数组保存字符串，不过没有使用 `final` 和 `private` 关键字修饰，最关键的是这个 `AbstractStringBuilder` 类还提供了很多修改字符串的方法。

每次对 `String` 类型进行改变的时候，都会生成一个新的 `String` 对象，然后将指针指向新的 `String` 对象。`StringBuffer` 每次都会对 `StringBuffer` 对象本身进行操作，而不是生成新的对象并改变对象引用。相同情况下使用 `StringBuilder` 相比使用 `StringBuffer` 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。

- 操作少量的数据: 适用 `String`
- 单线程操作字符串缓冲区下操作大量数据: 适用 `StringBuilder`
- 多线程操作字符串缓冲区下操作大量数据: 适用 `StringBuffer`

**链式编程：**在我们调用一个方法时，不需要用变量接收其结果，可以继续调用其他方法。

#### StringJoiner

构造方法：

- StringJoiner(间隔符号)    创建一个StringJoiner对象，指定拼接的间隔符号
- StringJoiner(间隔符号, 开始符号, 结束符号)    创建一个StringJoiner对象，指定......

成员方法：

- StringJoiner add(添加的内容)  添加数据吧，并返回对象本身
- int length()  返回长度
- String toString()  返回一个拼接后的结果字符串

#### String.intern

`String.intern()` 是一个 native（本地）方法，其作用是将指定的字符串对象的引用保存在字符串常量池中，可以保证相同内容的字符串变量引用同一个内存对象。

- 如果字符串常量池中保存了对应的字符串对象的引用，就直接返回该引用。
- 如果字符串常量池中没有保存了对应的字符串对象的引用，那就在常量池中创建一个指向该字符串对象的引用并返回。

#### 不可变的好处

**1. 可以缓存 hash 值**

因为 String 的 hash 值经常被使用，例如 String 用做 HashMap 的 key。不可变的特性可以使得 hash 值也不可变，因此只需要进行一次计算。

**2. String Pool 的需要**

如果一个 String 对象已经被创建过了，那么就会从 String Pool 中取得引用。只有 String 是不可变的，才可能使用 String Pool。

**3. 安全性**

String 经常作为参数，String 不可变性可以保证参数不可变。例如在作为网络连接参数的情况下如果 String 是可变的，那么在网络连接过程中，String 被改变，改变 String 对象的那一方以为现在连接的是其它主机，而实际情况却不一定是。

**4. 线程安全**

`String` 中的对象是不可变的，也就可以理解为常量，线程安全。`StringBuffer` 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。`StringBuilder` 并没有对方法进行加同步锁，所以是非线程安全的。

### 集合

和数组一样可以存储多个数据，但数组长度固定，集合长度可变。可以存引用数据类型、基本数据类型（不能直接存基本，需要变为包装类再存）。

#### ArrayList

泛型：限定集合中存储数据的类型 <引用数据类型>

构造方法例子：`ArrayList<String> list = new ArrayList<>();`

成员方法：

- boolean add(E e)  添加元素，返回值表示是否成功
- boolean remove(E e)  删除指定元素，返回值表示是否成功
- E remove(int index)  删除指定索引的元素，返回被删除的元素
- E set(int index, E e)  修改指定索引下的元素，返回原来的元素
- E get(int index)  获取指定索引的元素
- int size()  集合的长度

### 方法

方法重载：**同名**但参数类型或参数个数或参数顺序不同，与返回值无关。

基本数据类型变量中存储的是真实的数据；	引用数据类型变量中存储的是地址值，引用：使用了其他空间中的数据。

#### 方法的值传递

- **值传递**：方法接收的是实参值的拷贝，会创建副本。
- **引用传递**：方法接收的直接是实参所引用的对象在堆中的地址，不会创建副本，对形参的修改将影响到实参。

很多程序设计语言（比如 C++、 Pascal )提供了两种参数传递的方式，不过，在 Java 中只有值传递。

**为什么说 Java 只有值传递呢？** 

Java 对引用类型的参数采用的是并不是引用传递，这里传递的还是值，不过，这个值是实参的地址罢了！

**为什么 Java 不引入引用传递呢？**

## 面向对象

### 类和对象

- Javabean类：用来描述一类事物的类：不写main方法
- 测试类：可以在测试类中创建javabean类的对象并进行赋值调用：写main方法
- 工具类：不是用来描述一类事物的，而是帮助做一些事情的类

  1. 类名见名知意

  2. 私有化构造方法
  3. 方法定义为静态

​	补充：

1. 一个Java文件可以定义多个class类，但只能一个类是public修饰，且public修饰的类名必须是代码文件名。**建议实际开发中一个文件定义一个class类。**
2. 我们一直在不知不觉地使用构造方法，这也是为什么我们在创建对象的时候后面要加一个括号（因为要调用无参的构造方法）。如果我们重载了有参的构造方法，记得都要把无参的构造方法也写出来（无论是否用到），因为这可以帮助我们在创建对象的时候少踩坑。

- main方法：
  - public：被JVM调用，访问权限够大
  - static：被JVM调用，不用创建对象，直接类名访问。因为main方法是静态的，所以测试类中其他方法也需要是静态的
  - void：被JVM调用，不需要给JVM返回值
  - main：一个通用的名称，虽然不是关键字，但是被JVM识别
  - String[] args：以前用于接收键盘录入数据，现在没用了

- 对象数组
- static关键字：
  - 静态变量特点：
    - 被该类所有对象**共享**
    - 不属于对象，属于类
    - 随着类的加载而加载，优先于对象存在
    - 调用方式：类名调用（推荐）、对象名调用

  - 静态方法特点：
    - 多用于测试类和工具类中，Javabean类很少会用
    - 调用方式：类名调用（推荐）、对象名调用
    - 静态方法在类加载的时候就存在了，它不依赖于任何实例。所以静态方法必须有实现，也就是说它不能是抽象方法(abstract)
    - 只能访问所属类的静态字段和静态方法，方法中不能有 this 和 super 关键字
    
  - 静态语句块
    
  - 静态内部类
    
  - 静态导包：在使用静态变量和方法时不用再指明 ClassName，从而简化代码，但可读性大大降低
    
  - 初始化顺序（从先到后）：
    - 父类(静态变量、静态语句块)
    
    - 子类(静态变量、静态语句块)
    - 父类(实例变量、普通语句块)
    - 父类(构造函数)
    - 子类(实例变量、普通语句块)
    - 子类(构造函数)
    
  - 注意事项：
    - 静态方法只能访问静态变量和静态方法
    - 非静态方法可以访问静态变量或静态方法，也可以访问非静态
    - 静态方法中没有this关键字


### 封装

- 就近原则
- this关键字——区分成员变量和局部变量——本质：代表方法调用者（对象）的地址值
- MVC设计模式——SpringMVC --> 被取代
- 构造方法（方法名==类名）
  - 空参构造——默认值
  - 有参构造
  - 构造方法的重载 overload（不能被重写 override）
  - 无论使用与否，都手写无参构造，以及带全部参数的构造方法

#### 标准JavaBean类：

1. 类是公共的，类名要见名知意，且符合驼峰命名法
2. 成员变量要用private修饰
3. 提供至少两个构造方法
   - 无参构造方法
   - 带全部参数的构造方法

4. 成员方法
   - 提供每一个成员变量对应的setXxx() && getXxx()
   - 还有其他行为？也要写上！

### 继承

- 格式：class A extends B{ }
- 说明：子类继承就获取了父类中声明的所有成员变量和方法。
  - 构造方法不能被子类继承
  - 父类声明的private成员变量，子类也继承到了，只是因为封装性的影响，使得子类不能直接调用父类的结构
  - 子类可以继承父类中可以加载到虚方法表中的成员方法，否则不能
- 规定：
  - 一个类可以被多个子类继承；一个类只能有一个父类——JAVA中类的单继承性
  - 可以多层继承，直接父类，间接父类
  - 子类只能访问父类中非私有的成员
- 成员变量的访问特点：就近原则，先在局部位置找 -> 本类成员位置找 -> 父类成员位置找，逐级向上
- 成员方法的访问特点：
  - 直接调用：就近原则
  - super调用：访问父类

- 构造方法的访问特点：
  - 父类的构造方法不会被子类继承
  - 子类中所有的构造方法默认先访问父类中的无参构造，再执行自己。因为子类在初始化时有可能要用到父类中的数据，如果父类没有完成初始化子类将无法使用父类数据；子类初始化之前，一定要调用父类构造方法先完成父类数据空间的初始化。
  - 调用父类构造方法的方法：（子类构造方法的第一行语句默认为 super()，不写也存在）必须手动写super进行调用，且必须位于第一行


**Object类**：Java中所有类都直接或间接继承与Object类

**虚方法表**：非private，非static，非final

- this：理解为一个变量，表示当前方法调用者的地址值
- super：代表父类存储空间

- 重写 (Override / Overwrite)，本质上是在虚方法表中进行覆盖
  - 重写方法的名称、形参列表必须与父类的一致
  - 子类重写父类方法时，访问权限必须 >= 父类
  - 子类方法的返回类型必须是父类方法返回类型或为其子类型。
  - 建议**重写的方法尽量和父类保持一致**
  - 只有被添加到虚方法表中的方法可以被重写
-  重载 (Overload)：存在于同一个类中，指一个方法与已经存在的方法名称上相同，但是参数类型、个数、顺序至少有一个不同。注意：返回值不同，其它都相同不算是重载。


### 多态

同类型的对象，表现出不同的形态——将子类对象赋值给父类类型

前提：

- 有继承或实现关系
- 有父类引用指向子类对象
- 有方法重写

调用成员特点：

- 变量调用：
  - 编译看左边：javac编译代码时，会看左边的父类中有没有这个变量，如果有，编译成功，如果没有，编译失败
  - 运行看左边：java运行代码时，实际获取的就是左边父类中成员变量的值
- 方法调用：
  - 编译看左边
  - 运行看右边

多态的弊端：不能调用子类的特有功能        解决方案：变回子类类型——变量名 instanceof 类名

#### final关键字

- 修饰方法：不能被重写
- 修饰类：不能被继承
- 修饰变量：叫做常量，只能被赋值一次
  - 基本数据类型的变量：数据值不能发生改变
  - 引用类型的变量：地址值不能发生改变，对象内部的属性值可以改变

### 权限修饰符

修饰符	同个类中	同个包中其他类	不同包下的子类	不同包下的无关类

priavate	√

空着不写	√						√

protected	√						√							√

public			√						√							√							√

### 代码块

- 局部代码块（已淘汰）
- 构造代码块
- 静态代码块 `static{}`

### 抽象类和抽象方法

abstract

- 抽象类不能实例化
- 抽象类中不一定有抽象方法，有抽象方法的类一定是抽象类
- 抽象类可以有构造方法
- 抽象类的子类：要么重写抽象类中的所有抽象方法，要么还是抽象类

### 接口

接口就是一种规则，是对行为的抽象。

- 接口定义用interface
- 接口不能实例化
- 接口和类型直接是实现关系，通过implements关键字
- 接口的子类（实现类）要么重写接口中所有抽象方法，要么也是抽象类
- 接口和类是实现关系，可以单实现，也可以多实现；实现类还可以在继承一个类的同时实现多个接口

接口的应用：

- 想让哪个类拥有一个行为，让这个类实现对应的接口就可以了
- 当一个方法的参数是接口时，可以传递接口所有实现类的对象，这种方式被称为接口多态

接口中成员的特点：

- 成员变量：
  - 只能是常量，默认修饰符为 public static final
- 构造方法
  - 没有
- 成员方法
  - JDK7以前：只能是抽象方法，默认修饰符 public abstract
  - JDK8新特性：接口中可以定义有方法体的方法
  - JDK9新特性：接口中可以定义私有方法

接口和接口的关系：是继承关系，可以单继承也可以多继承。	注：如果实现类实现了最底部的子接口，那么就需要重写所有的抽象方法。

#### 接口新增方法

JDK8. 允许在接口中定义**默认方法**，用关键字default修饰

作用：解决接口升级问题

注意：

- 默认方法不是抽象方法，所以不强制被重写。但是如果被重写，重写时去掉default关键字
- public可以省略，default不能省略
- 如果实现了多个接口，多个接口中存在相同名字的默认方法，子类就必须对该方法进行重写



JDK8. 允许在接口中定义**静态方法**，用关键字static修饰

注意：

- 静态方法只能通过接口名调用，不能通过实现类名或对象名调用
- public可以省略，static不能省略



JDK9. 允许在接口中定义**私有方法**，用关键字private修饰，分为普通的私有方法（为默认方法服务）、静态的私有方法（为静态方法服务）

#### 适配器设计模式

**设计模式**：就是解决各种问题的套路

解决接口与接口实现之间的矛盾问题

### 内部类

​	在一个类内部再定义一个类

类的五大成员：属性、方法、构造方法、代码块、内部类

内部类表示的事物是外部类的一部分。

访问特点：

- 内部类可以直接访问外部类的成员，包括私有
- 外部类要访问内部类的成员，必须创建对象

#### 成员内部类

#### 静态内部类

静态内部类只能访问外部类中的静态变量和静态方法，如果想要访问非静态的需要创建对象

#### 局部内部类

将内部类定义在方法里面

#### 匿名内部类

格式：

```java
new 类名或接口名() {
    重写方法;
};
```

包含：实现/继承、方法的重写、创建对象

- 可以写在成员位置，也可以写在局部位置

- 当方法的参数是接口或者类时，可以当作参数传递，用匿名内部类简化代码

## 常用API

查阅JDK API

### Math

进行数学计算的工具类

### System

提供与系统相关方法的工具类

### Runtime

当前虚拟机的运行环境的工具类

`getRuntime()`

### Object

Object 类是一个特殊的类，是所有类的父类。顶级父类 Object 只有无参构造方法。

#### 主要方法

```java
/**
 * native 方法，用于返回当前运行时对象的 Class 对象，使用了 final 关键字修饰，故不允许子类重写。
 */
public final native Class<?> getClass()
/**
 * native 方法，用于返回对象的哈希码，主要使用在哈希表中，比如 JDK 中的HashMap。
 */
public native int hashCode()
/**
 * 用于比较 2 个对象的内存地址是否相等，String 类对该方法进行了重写以用于比较字符串的值是否相等。
 */
public boolean equals(Object obj)
/**
 * native 方法，用于创建并返回当前对象的一份拷贝。
 */
protected native Object clone() throws CloneNotSupportedException
/**
 * 返回类的名字实例的哈希码的 16 进制的字符串。建议 Object 所有的子类都重写这个方法。
 */
public String toString()
/**
 * native 方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。
 */
public final native void notify()
/**
 * native 方法，并且不能重写。跟 notify 一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。
 */
public final native void notifyAll()
/**
 * native方法，并且不能重写。暂停线程的执行。注意：sleep 方法没有释放锁，而 wait 方法释放了锁 ，timeout 是等待时间。
 */
public final native void wait(long timeout) throws InterruptedException
/**
 * 多了 nanos 参数，这个参数表示额外时间（以纳秒为单位，范围是 0-999999）。 所以超时的时间还需要加上 nanos 纳秒。。
 */
public final void wait(long timeout, int nanos) throws InterruptedException
/**
 * 跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念
 */
public final void wait() throws InterruptedException
/**
 * 实例被垃圾回收器回收的时候触发的操作
 */
protected void finalize() throws Throwable { }

```

#### == 和 equals() 的区别

**`==`** 对于基本类型和引用类型的作用效果是不同的：

- 对于基本数据类型来说，`==` 比较的是值。
- 对于引用数据类型来说，`==` 比较的是对象的内存地址。

*因为 Java 只有值传递，所以，对于 == 来说，不管是比较基本数据类型，还是引用数据类型的变量，其本质比较的都是值，只是引用类型变量存的值是对象的地址。*

**`equals()`** 不能用于判断基本数据类型的变量，只能用来判断两个对象是否相等。`equals()`方法存在于`Object`类中，而`Object`类是所有类的直接或间接父类，因此所有的类都有`equals()`方法。

`equals()` 方法存在两种使用情况：

- **类没有重写 `equals()`方法**：通过`equals()`比较该类的两个对象时，等价于通过“==”比较这两个对象，使用的默认是 `Object`类`equals()`方法。
- **类重写了 `equals()`方法**：一般我们都重写 `equals()`方法来比较两个对象中的属性是否相等；若它们的属性相等，则返回 true(即，认为这两个对象相等)。

注：`String` 中的 `equals` 方法是被重写过的

#### hashCode()

`hashCode()` 的作用是获取哈希码（`int` 整数），也称为散列码。这个哈希码的作用是确定该对象在哈希表中的索引位置。

`hashCode()` 定义在 JDK 的 `Object` 类中，这就意味着 Java 中的任何类都包含有 `hashCode()` 函数。另外需要注意的是：`Object` 的 `hashCode()` 方法是本地方法，也就是用 C 语言或 C++ 实现的。

*⚠️ 注意：该方法在 **Oracle OpenJDK8** 中默认是 "使用线程局部状态来实现 Marsaglia's xor-shift 随机数生成", 并不是 "地址" 或者 "地址转换而来", 不同 JDK/VM 可能不同在 **Oracle OpenJDK8** 中有六种生成方式 (其中第五种是返回地址), 通过添加 VM 参数: -XX:hashCode=4 启用第五种。参考源码。*

散列表存储的是键值对(key-value)，它的特点是：**能根据“键”快速的检索出对应的“值”。这其中就利用到了散列码！（可以快速找到所需要的对象）**

#### 对象克隆

​	把A对象的属性值完全拷贝给B对象的方法：`protected Object clone()`

**需要重写 clone() 方法，实现 Cloneable 接口（是一个标记性接口，表示如果实现了，那么当前类的对象可以被克隆）**

**浅克隆：**克隆对象和原始对象的引用类型引用同一个对象

**深克隆：**克隆对象和原始对象的引用类型引用不同对象

**clone() 的替代方案：**使用 clone() 方法来拷贝一个对象即复杂又有风险，它会抛出异常，并且还需要类型转换。Effective Java 书上讲到，最好不要去使用 clone()，可以使用拷贝构造函数或者拷贝工厂来拷贝一个对象。

#### 对象工具类 Objects

是一个工具类，提供一些方法完成一些功能

### BigInteger

大整数，只能是整数，取值范围近乎无限大

**对象一旦创建，内部记录的值不能发生改变，只要进行计算都会产生一个新的BigInteger对象。**

### BigDecima

《阿里巴巴 Java 开发手册》中提到：“1. 为了避免精度丢失，可以使用 `BigDecimal` 来进行浮点数的运算。2. **浮点数之间的等值判断，基本数据类型不能用 == 来比较，包装数据类型不能用 equals 来判断。**”

通常情况下，大部分需要浮点数精确运算结果的业务场景（比如涉及到钱的场景）都是通过 `BigDecimal` 来做的。

1. 想要解决浮点数运算精度丢失这个问题，可以直接使用 `BigDecimal` 来定义浮点数的值，然后再进行浮点数的运算操作即可。
2. 因为 `equals()` 方法不仅仅会比较值的大小（value）还会比较精度（scale），而 `compareTo()` 方法比较的时候会忽略精度。

#### BigDecimal 工具类分享

```java
import java.math.BigDecimal;
import java.math.RoundingMode;

/**
 * 简化BigDecimal计算的小工具类
 */
public class BigDecimalUtil {

    /**
     * 默认除法运算精度
     */
    private static final int DEF_DIV_SCALE = 10;

    private BigDecimalUtil() {
    }

    /**
     * 提供精确的加法运算。
     *
     * @param v1 被加数
     * @param v2 加数
     * @return 两个参数的和
     */
    public static double add(double v1, double v2) {
        BigDecimal b1 = BigDecimal.valueOf(v1);
        BigDecimal b2 = BigDecimal.valueOf(v2);
        return b1.add(b2).doubleValue();
    }

    /**
     * 提供精确的减法运算。
     *
     * @param v1 被减数
     * @param v2 减数
     * @return 两个参数的差
     */
    public static double subtract(double v1, double v2) {
        BigDecimal b1 = BigDecimal.valueOf(v1);
        BigDecimal b2 = BigDecimal.valueOf(v2);
        return b1.subtract(b2).doubleValue();
    }

    /**
     * 提供精确的乘法运算。
     *
     * @param v1 被乘数
     * @param v2 乘数
     * @return 两个参数的积
     */
    public static double multiply(double v1, double v2) {
        BigDecimal b1 = BigDecimal.valueOf(v1);
        BigDecimal b2 = BigDecimal.valueOf(v2);
        return b1.multiply(b2).doubleValue();
    }

    /**
     * 提供（相对）精确的除法运算，当发生除不尽的情况时，精确到
     * 小数点以后10位，以后的数字四舍五入。
     *
     * @param v1 被除数
     * @param v2 除数
     * @return 两个参数的商
     */
    public static double divide(double v1, double v2) {
        return divide(v1, v2, DEF_DIV_SCALE);
    }

    /**
     * 提供（相对）精确的除法运算。当发生除不尽的情况时，由scale参数指
     * 定精度，以后的数字四舍五入。
     *
     * @param v1    被除数
     * @param v2    除数
     * @param scale 表示表示需要精确到小数点以后几位。
     * @return 两个参数的商
     */
    public static double divide(double v1, double v2, int scale) {
        if (scale < 0) {
            throw new IllegalArgumentException(
                    "The scale must be a positive integer or zero");
        }
        BigDecimal b1 = BigDecimal.valueOf(v1);
        BigDecimal b2 = BigDecimal.valueOf(v2);
        return b1.divide(b2, scale, RoundingMode.HALF_EVEN).doubleValue();
    }

    /**
     * 提供精确的小数位四舍五入处理。
     *
     * @param v     需要四舍五入的数字
     * @param scale 小数点后保留几位
     * @return 四舍五入后的结果
     */
    public static double round(double v, int scale) {
        if (scale < 0) {
            throw new IllegalArgumentException(
                    "The scale must be a positive integer or zero");
        }
        BigDecimal b = BigDecimal.valueOf(v);
        BigDecimal one = new BigDecimal("1");
        return b.divide(one, scale, RoundingMode.HALF_UP).doubleValue();
    }

    /**
     * 提供精确的类型转换(Float)
     *
     * @param v 需要被转换的数字
     * @return 返回转换结果
     */
    public static float convertToFloat(double v) {
        BigDecimal b = new BigDecimal(v);
        return b.floatValue();
    }

    /**
     * 提供精确的类型转换(Int)不进行四舍五入
     *
     * @param v 需要被转换的数字
     * @return 返回转换结果
     */
    public static int convertsToInt(double v) {
        BigDecimal b = new BigDecimal(v);
        return b.intValue();
    }

    /**
     * 提供精确的类型转换(Long)
     *
     * @param v 需要被转换的数字
     * @return 返回转换结果
     */
    public static long convertsToLong(double v) {
        BigDecimal b = new BigDecimal(v);
        return b.longValue();
    }

    /**
     * 返回两个数中大的一个值
     *
     * @param v1 需要被对比的第一个数
     * @param v2 需要被对比的第二个数
     * @return 返回两个数中大的一个值
     */
    public static double returnMax(double v1, double v2) {
        BigDecimal b1 = new BigDecimal(v1);
        BigDecimal b2 = new BigDecimal(v2);
        return b1.max(b2).doubleValue();
    }

    /**
     * 返回两个数中小的一个值
     *
     * @param v1 需要被对比的第一个数
     * @param v2 需要被对比的第二个数
     * @return 返回两个数中小的一个值
     */
    public static double returnMin(double v1, double v2) {
        BigDecimal b1 = new BigDecimal(v1);
        BigDecimal b2 = new BigDecimal(v2);
        return b1.min(b2).doubleValue();
    }

    /**
     * 精确对比两个数字
     *
     * @param v1 需要被对比的第一个数
     * @param v2 需要被对比的第二个数
     * @return 如果两个数一样则返回0，如果第一个数比第二个数大则返回1，反之返回-1
     */
    public static int compareTo(double v1, double v2) {
        BigDecimal b1 = BigDecimal.valueOf(v1);
        BigDecimal b2 = BigDecimal.valueOf(v2);
        return b1.compareTo(b2);
    }

}

```

### 正则表达式

```java
// 获取正则表达式的对象
Pattern p = Pattern.compile("");
// 获取文本匹配器的对象
// m:文本匹配器对象
// str:大串
// p:规则
// m要在str中找符合p的小串
Matcher m = p.matcher(str);
// 依照文本匹配器从头开始读取，寻找是否有满足规则的子串
// 如果没有，返回false
// 如果有，返回true，在底层记录子串的起始索引和结束索引+1
boolean b = m.find();
```

#### 作用

1. 校验字符串是否满足规则
2. 在一段文本中查找满足要求的内容——爬虫

#### 分组

就是一个小括号

**捕获分组：**把该组的数据捕获出来，再用一次。

后续还要继续使用本组的数据

正则内部使用：`\\组号`，正则外部使用：`$组号`

**非捕获分组：**分组之后不需要再用本组数据，仅仅是把数据括起来，不占组号。

### 时间类

- JDK7前
  - Date
  - SimpleDateFormat
  - Calendar
- JDK8新增——更简单、更安全
  - Date类
    - ZoneId：时区
    - Instant：时间戳
    - ZoneDateTime：带时区的时间
  - 日期格式化类
    - DateTimeFormatter：用于时间的格式化和解析
  - 日历类
    - LocalDate：年月日
    - LocalTime：时分秒
    - LocalDateTime：年月日时分秒
  - 工具类
    - Duration：时间间隔（秒，纳秒）
    - Period：时间间隔（年月日）
    - ChronoUnit：时间间隔（所有单位）

### Unsafe

JUC 源码中，很多并发工具类都调用了`Unsafe` 类

Unsafe 主要提供一些用于执行低级别、不安全操作的方法，如直接访问系统内存资源、自主管理内存资源等，这些方法在提升 Java 运行效率、增强 Java 语言底层资源操作能力方面起到了很大的作用。但由于 `Unsafe` 类使 Java 语言拥有了类似 C 语言指针一样操作内存空间的能力，这无疑也增加了程序发生相关指针问题的风险。在程序中过度、不正确使用 `Unsafe` 类会使得程序出错的概率变大，使得 Java 这种安全的语言变得不再“安全”，因此对 `Unsafe` 的使用一定要慎重。

另外，`Unsafe` 提供的这些功能的实现需要依赖本地方法（Native Method）。可以将本地方法看作是 Java 中使用其他编程语言编写的方法。本地方法使用 **`native`** 关键字修饰，Java 代码中只是声明方法头，具体的实现则交给 **本地代码**。

### Arrays

操作数组的工具类

### Lambda表达式

注意点：

- Lambda表达式可以用来简化匿名内部类的书写
- Lambda表达式只能简化**函数式接口**的匿名内部类的写法
  - 函数式接口：**有且仅有一个抽象方法**的**接口**叫做函数式接口，接口上方可以加@FunctionalInterface 注解

格式：

```java
方法名(
    ()->{
        重写接口的抽象方法;
    }
);
```

省略规则：

- 参数类型可以省略不写
- 如果只有一个参数，参数类型可以省略，同时 () 也可以省略
- 如果Lambda表达式的方法体只有一行，大括号、分号、return 可以省略不写，必须同时省略

## 泛型

适用于多种数据类型执行相同的代码（代码复用）  <数据类型>

可以在编译阶段约束操作的数据类型，并进行检查，如果传入其他类型的对象就会报错。泛型中的类型在使用时指定，不需要强制类型转换（**类型安全**，编译器会**检查类型**）。并且，原生 `List` 返回类型是 `Object` ，需要手动转换类型才能使用，使用泛型后编译器自动转换。

Java中的泛型是伪泛型：

- 只能支持引用数据类型
- 指定具体类型后，传递数据时，可以传入该类类型或者其子类类型
- 如果不写泛型，类型默认是Object

**使用方式：**

- 类后面——泛型类：`修饰符 class 类名<类型T>{}`	（方法中多个形参不确定）
- 方法上面——泛型方法：`修饰符<类型E>返回值类型 方法名(类型 变量名){}`        （方法中一个形参不确定）
- 接口后面——泛型接口：`修饰符 interface 接口名<类型T>{}`

#### 泛型的继承

泛型不具备继承性，但是数据具备继承性。

#### 泛型的通配符

? ：表示不确定的类型，但是可以进行类型的限定

- ? extends E：可以传递E或E所有的子类类型
- ? super E：可以传递E或E所有的父类类型

**应用场景：**若类型不确定，但想要只能传递某个继承体系中的类型

### 泛型类

在实例化泛型类时，必须指定 T 的具体类型。

### 泛型接口

实现泛型接口：

1. 实现类给出具体类型
2. 实现类延续泛型，创建对象时再确定

### 泛型方法

泛型方法，是在调用方法的时候指明泛型的具体类型，可以用`Class.forName()`作为参数指定泛型的具体类型。

**注意：** `public static < E > void printArray( E[] inputArray )` 一般被称为静态泛型方法;在 java 中泛型只是一个占位符，必须在传递类型后才能使用。类在实例化时才能真正的传递类型参数，由于静态方法的加载先于类的实例化，也就是说类中的泛型还没有传递真正的类型参数，静态的方法的加载就已经完成了，所以静态泛型方法是没有办法使用类上声明的泛型的。只能使用自己声明的 <E>

#### 为什么要使用泛型方法呢？

因为泛型类要在实例化的时候就指明类型，如果想换一种类型，不得不重新new一次，可能不够灵活；而泛型方法可以在调用的时候指明类型，更加灵活。

### 泛型数组

泛型数组相关的声明：

```java
List<String>[] list11 = new ArrayList<String>[10]; //编译错误，非法创建 
List<String>[] list12 = new ArrayList<?>[10]; //编译错误，需要强转类型 
List<String>[] list13 = (List<String>[]) new ArrayList<?>[10]; //OK，但是会有警告 
List<?>[] list14 = new ArrayList<String>[10]; //编译错误，非法创建 
List<?>[] list15 = new ArrayList<?>[10]; //OK 
List<String>[] list6 = new ArrayList[10]; //OK，但是会有警告
```

### 项目中哪里用到了泛型？

- 自定义接口通用返回结果 `CommonResult<T>` 通过参数 `T` 可根据具体的返回类型动态指定结果的数据类型

- 定义 `Excel` 处理类 `ExcelUtil<T>` 用于动态指定 `Excel` 导出的数据类型

- 构建集合工具类（参考 `Collections` 中的 `sort`, `binarySearch` 方法）。

- ……

## 异常

在 Java 中，所有的异常都有一个共同的祖先 `java.lang` 包中的 `Throwable` 类。`Throwable` 类有两个重要的子类:

- **`Exception`** :程序本身可以处理的异常，可以通过 `catch` 来进行捕获。`Exception` 又可以分为 Checked Exception (受检查异常，必须处理) 和 Unchecked Exception (不受检查异常，可以不处理)。
- **`Error`**：`Error` 属于程序无法处理的错误

**运行时异常 RuntiomeException**

在编译阶段不需要处理，是代码运行阶段出现的异常。

**Unchecked Exception** 即 **不受检查异常** ，Java 代码在编译过程中 ，我们即使不处理不受检查异常也可以正常通过编译。`RuntimeException` 及其子类都统称为非受检查异常

**编译时异常 Checked Exception**

受检查异常 ，在编译阶段，必须手动处理，Java 代码在编译过程中，如果受检查异常没有被 `catch`或者`throws` 关键字处理的话，就没办法通过编译，代码报错。

除了`RuntimeException`及其子类以外，其他的`Exception`类及其子类都属于受检查异常 。常见的受检查异常有：IO 相关的异常、`ClassNotFoundException`、`SQLException`...

### 作用

1. 用来查询bug的关键参考信息
2. 作为方法内部的一种特殊返回值，用来通知调用者底层的执行情况

### 处理方式

1. JVM默认的：把异常的名称，异常的原因及异常出现的位置等信息输出在控制台。此时程序停止运行，下面的代码不再执行。
2. 自己处理（捕获异常）：`try{可能出现异常的代码} catch(异常类名 变量名){异常的处理代码;}` 。当代码出现异常时，程序可以继续执行。
   1. try中没有遇到问题，怎么执行？——会执行try中的所有代码，不会执行catch中的代码。
   2. try中遇到多个问题，怎么处理？——写多个catch与之对应；**如果这些异常存在父子关系，那么父类一定要写在下面。**
   3. try中遇到的问题没有被捕获，怎么执行？——交给JVM进行默认处理。
   4. try中遇到问题，try中下面的剩余代码还会执行吗？——不会。会直接跳转至对应的catch中执行catch内的语句体，但如果没有对应的catch与之匹配，那么会交给JVM默认处理。
3. 抛出异常：
   - throws：写在方法定义处，表示声明一个异常，告诉调用者，使用本方法可能会有哪些异常（编译时异常：必须写，运行时异常：可以不写）
   - throw：写在方法内，结束方法。手动抛出异常对象，交给调用者，方法中下面的代码不再执行。

#### try……catch异常处理

- `try`块：用于捕获异常。其后可接零个或多个 `catch` 块，如果没有 `catch` 块，则必须跟一个 `finally` 块。

- `catch`块：用于处理 try 捕获到的异常。

- `finally` 块：无论是否捕获或处理异常，`finally` 块里的语句都会被执行，除非虚拟机停止。当在 `try` 块或 `catch` 块中遇到 `return` 语句时，`finally` 语句块将在方法返回之前被执行。
  - **注意：不要在 finally 语句块中使用 return!** 当 try 语句和 finally 语句中都有 return 语句时，try 语句块中的 return 语句会被忽略。这是因为 try 语句中的 return 返回值会先被暂存在一个本地变量中，当执行到 finally 语句中的 return 之后，这个本地变量的值就变为了 finally 语句中的 return 返回值。

#### 常见方法

**Throwable 的成员方法**

`String getMessage()`  返回异常发生时的简要描述

`String toString()`  返回异常发生时的详细信息

`String getLocalizedMessage()`  返回异常对象的本地化信息。使用 `Throwable` 的子类覆盖这个方法，可以生成本地化信息。如果子类没有覆盖该方法，则该方法返回的信息与 `getMessage()`返回的结果相同

`void printStackTrace()`  在控制台上打印 `Throwable` 对象封装的异常信息（仅打印信息，不会停止程序运行）

### 自定义异常

意义：让控制台的报错信息更加见名知意

步骤：

1. 定义异常类
2. 写继承关系
3. 空参构造
4. 带参构造

## 文件 File

File对象就表示一个路径，可以是文件的路径，也可以是文件夹的路径。这个路径可以是存在的，也允许是不存在的。

**构造方法**

`public File(String pathname)`  根据文件路径创建文件对象

`public File(String parent, String child)`  根据父路径名字符串和子路径名字符串创建文件对象

`public File(File parent, String child) ` 根据父路径对应文件对象和子路径名字符串创建文件对象

### 成员方法

通过API直接查😋

#### 判断

#### 获取

#### 创建

#### 删除

- 如果删除的是文件，则直接删除不进回收站；如果删除的是空文件夹，直接删除不进回收站；如果删除的是有内容的文件夹，则删除失败。

#### 获取并遍历

`public File[] listFiles()` 获取当前路径下所有内容（文件和文件夹，包含隐藏文件），把所有内容放入数组中返回

- 当路径不存在，返回null
- 当表示的路径是文件，返回null
- 当表示的路径是一个空文件夹，返回一个长度为0的数组
- 当表示的路径是一个需要权限才能访问的文件夹时，返回null

还有其他的仅作了解

## SPI

SPI（Service Provider Interface），是JDK内置的一种 服务提供发现机制，可以用来启用框架扩展和替换组件，主要是被框架的开发人员使用。Java的SPI机制可以为某个接口寻找服务实现。

面向对象设计鼓励模块间基于接口而非具体实现编程，以降低模块间的耦合，遵循依赖倒置原则，并支持开闭原则（对扩展开放，对修改封闭）。然而，直接依赖具体实现会导致在替换实现时需要修改代码，违背了开闭原则。为了解决这个问题，SPI 应运而生，它提供了一种服务发现机制，允许在程序外部动态指定具体实现。这与控制反转（IoC）的思想相似，将组件装配的控制权移交给了程序之外，其核心思想就是**解耦**。

### SPI 和 API 的区别

说到 SPI 就不得不说一下 API 了，从广义上来说它们都属于接口，而且很容易混淆。

一般模块之间都是通过接口进行通讯，那我们在服务调用方和服务实现方（也称服务提供者）之间引入一个“接口”。

当实现方提供了接口和实现，我们可以通过调用实现方的接口从而拥有实现方给我们提供的能力，这就是 API ，这种接口和实现都是放在实现方的。

当接口存在于调用方这边时，就是 SPI ，由接口调用方确定接口规则，然后由不同的厂商去根据这个规则对这个接口进行实现，从而提供服务。

### 优缺点

通过 SPI 机制能够大大地提高接口设计的灵活性，但是 SPI 机制也存在一些缺点，比如：

- 需要遍历加载所有的实现类，不能做到按需加载，这样效率还是相对较低的。
- 当多个 `ServiceLoader` 同时 `load` 时，会有并发问题。

## 语法糖

**语法糖（Syntactic sugar）** 代指的是编程语言为了方便程序员开发程序而设计的一种特殊语法，这种语法对编程语言的功能并没有影响。实现相同的功能，基于语法糖写出来的代码往往更简单简洁且更易阅读。例如，Java 中的 `for-each` 就是一个常用的语法糖，其原理其实就是基于普通的 for 循环和迭代器。

不过，JVM 其实并不能识别语法糖，Java 语法糖要想被正确执行，需要先通过编译器进行解糖，也就是在程序编译阶段将其转换成 JVM 认识的基本语法。这也侧面说明，Java 中真正支持语法糖的是 Java 编译器而不是 JVM。如果去查看`com.sun.tools.javac.main.JavaCompiler`的源码，会发现在`compile()`中有一个步骤就是调用`desugar()`，这个方法就是负责解语法糖的实现的。

### 常见的语法糖

Java 中最常用的语法糖主要有泛型、自动拆装箱、变长参数、枚举、内部类、增强 for 循环、try-with-resources 语法、lambda 表达式等。
