# C语言

## 存储类

​	定义C程序中变量 / 函数的存储位置、生命周期和作用域

#### auto

所有局部变量默认的存储类。意味着它们在函数开始时被创建，在函数结束时被销毁。（auto仅能用于函数内，即只能修饰局部变量）

#### register

用于定义存储在寄存器而非RAM中的局部变量，最大尺寸等于寄存器大小，所以变量访问速度更快，但不能直接取址&，因为没有内存位置。

#### △static

指示编译器在程序生命周期内保持局部变量存在，无需在它每次进入 / 离开作用域时创建 / 销毁。因此static修饰的局部变量可以在函数调用之间保持值，也可以应用于全局变量，但会使作用域限制在声明它的文件内。静态变量只初始化一次，即使函数被调用多次，该变量的值也不会重置。

#### △extern

用于定义在其他文件中声明的全局变量或函数。不会为被其修饰的变量分配任何存储空间，而是只是指示编译器该变量在其他文件中定义。提供了一个全局变量的引用，使用后对于一个无法初始化的变量，会把变量名指向一个之前定义的存储位置。当有多个文件且定义了一个可在其他文件中使用的全局变量或函数时，可以在另一文件中使用extern来得到已定义的引用。

### 初始化

当局部变量被定义时，系统不会对其初始化，必须自行初始化；定义全局变量后，系统会对其自动初始化，如下：

int --> 0，char --> '\0'，float --> 0，double --> 0，pointer --> NULL

## enum 枚举

​	用于定义一组具有离散值的常量，使数据更加简洁可读

enum 枚举名 {元素1, 元素2, ...}

例如：

enum WEEK

{

​	MON = 1, TUE, WED, THU, FRI, SAT, SUN

};

注：第一个枚举成员默认值为整型的0，后续枚举成员的值在前一个上加1。

#### 定义枚举变量

1.enum WEEK week;

2.enum WEEK{

}week;

3.enum{

}week;	（省略枚举名称）

#### 使用枚举变量

enum WEEK week;

week = MON;



枚举类型被C语言当做int或者unsigned（int类型无法遍历，而枚举元素连续则可用for遍历）

#### 类型转换

int a；

enum WEEK b;

b = (enum WEEK) a;



<font color='blue'>字符串实际上是使用空字符‘\0’结尾的一维字符数组</font>



## 结构体 struct

C数组允许定义可存储相同类型数据项的变量，第一个元素地址即为数组地址。而结构体允许存储不同类型。

#### 定义结构体

struct tag{

​	member_list1	// 标准的变量定义

​	member_list2

​	…

}variable_list;	// 结构变量定义，可指定一个或多个	p.s. 别忘了进行初始化

#### 引用

tag.member	// 访问成员运算符'.'

使用struct关键字定义结构类型变量

strcpy()函数——包含于标准库<string.h>中

#### 指向结构的指针

定义：`struct tag * struct_pointer;`

存储结构变量地址：`struct_pointer = &variable;`

使用指向该结构体的指针访问结构成员：`struct_pointer->member;`

## 共用体 union

​	类似于结构体

……

## 指针

​	指针就是内存地址，指针变量是用来存放地址的变量。  type *var_name;

在变量声明的时候，如果没有确切的地址可以赋值，则为变量赋一个NULL值是一个良好的编程习惯，NULL空指针的地址是0x0。

#### malloc  动态内存分配

内存位于堆中，没有初始化内存内容。无法知道内存具体位置时使用。

#### calloc

将初始化内存设置为0。

#### realloc

对申请的内存大小进行调整，申请的内存需通过free()释放。



栈存放：函数相关参数、返回地址、局部变量、寄存器内容

堆存放：程序员填充

#### 指针与函数

函数指针可以作为某个函数的参数，回调函数就是一个通过函数指针调用的函数

## 标准库

见《C程序设计语言》P241

## GCC编译器

​	-s：看C语言编译器产生的汇编代码

​	-c：编译并汇编目标代码	.c -> .o

​	objdump -d  目标.o  反汇编

GDB调试器