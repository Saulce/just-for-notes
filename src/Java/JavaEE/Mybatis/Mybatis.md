# Mybatis框架

​	持久层（数据访问层）框架，用于简化JDBC的开发。JDBC就是使用Java语言操作关系型数据库的一套API。

1. 创建springboot工程、数据库表、实体类

2. 引入mybatis相关依赖，配置mybatis（数据库连接信息）

   ```properties
   spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
   spring.datasource.url=jdbc:mysql://localhost:3306/mybatis
   spring.datasource.username=root
   spring.datasource.password=
   ```

3. 编写SQL语句（注解 / XML） `@Mapper`

   - 配置SQL提示

     - 产生原因：idea和数据库没有建立连接，不识别表的信息

     - 解决方式：在idea中配置MySQL数据库连接

### JDBC

- Java DataBase Connectivity ，使用 Java 语言操作关系型数据库的一套API 。

- sun公司官方定义的一套操作所有关系型数据库的规范，即接口。
- 由各个数据库厂商实现这套接口，提供数据库**驱动 jar 包**。
- 使用 JDBC 编程，真正执行代码的是驱动 jar 包中的实现类。

缺点：

- 硬解码

- 繁琐

- 资源浪费，性能降低

——被Mybatis **取代**

### 数据库连接池

- 是个容器，负责分配、管理数据库连接(Connection)。
- 类似于线程池；它允许应用程序重复使用一个现有的数据库连接，而不是重新建立一个。
- 通过释放空闲时间超过最大空闲时间的连接，来避免因为没有释放连接而引起的数据库连接遗漏。

标准接口：DataSource ：官方提供的数据库连接池接口，由第三方实现此接口；功能：获取连接

- **Hikari**——Spring Boot默认的连接池

- **Druid** 连接池是阿里巴巴开源的数据库连接池项目，功能强大性能优秀，是 Java语言最好的数据库连接池之一
- C3P0
- DBCP
- ……

切换连接池：引入依赖，配置连接信息。

### lombok

Lombok是一个实用的Java类库，能通过注解的形式自动生成构造器、getter/setter、equals、hashcode、toString等方法，并可以自动化生成日志变量，简化java开发，提高效率。

注解：

- @Data ： 注在类上，可以⽣成⽆参构造和类⾥⾯所有属性的getter/setter⽅法、equals、hashCode、canEqual、toString方法。
  - @Data 注解的主要作用是提高代码的简洁，使用这个注解可以省去代码中大量的get()、 set()、 toString()等方法。
  - 那么怎么自动生成有参构造器呢？使用@Builder注解，将会帮助我们⽣成全属性的构造⽅法。但是如果只引用@Builder注解是无法生成get和set的。但是如果同时使⽤@Data和@Builder的话，可以看出尽管⽣成了GET/SET⽅法，但是⽆参构造⽅法没有了，这显然是不能接受的，因为很多框架都会调⽤⽆参构造去创建对象。
- @Builder ：注在类、构造函数或方法上，为你的类生成相对略微复杂的构建器API。简言之，就是一步步创建一个对象，它对用户屏蔽了里面构建的细节，但可以精细地控制对象的构造过程
  - 一个名为FooBuilder的内部静态类，并具有和实体类形同的属性（称为构建器）
  - 在构建器中：对于目标类中的所有的属性和未初始化的final字段，都会在构建器中创建对应属性
  - 在构建器中：创建一个无参的default构造函数
  - 在构建器中：对于实体类中的每个参数，都会对应创建类似于“setter”的方法，只不多方法名与该参数名相同。 并且返回值是构建器本身（便于链式调用）
  - 在构建器中：一个build()方法，调用此方法，就会根据设置的值进行创建实体对象
  - 在构建器中：同时也会生成一个toString()方法
  - 在实体类中：会创建一个builder()方法，它的目的是用来创建构建器
- @AllArgsConstructor ： 注在类上，提供类的全参构造
- @NoArgsConstructor ： 注在类上，提供类的无参构造
- @Setter ： 注在属性上，提供 set 方法
- @Getter ： 注在属性上，提供 get 方法
- @EqualsAndHashCode ： 注在类上，提供对应的 equals 和 hashCode 方法
- @Log4j/@Slf4j ： 注在类上，提供对应的 Logger 对象，变量名为 log

依赖：

```java
<dependency>
	<groupId>org.projectlombok</groupId>
	<artifictId>lombok</artifictId>
</dependency>
```

## 常见操作

- mapper
- pojo

### 删除

map接口中，方法之上： `@Delete("sql语句")`

#### 预编译SQL

日志输出：`mybatis.configuration.log-impl=org.apache.ibatis.stdout.stdOutImpl`  可以在applicton.properties 中打开 mybatis 日志，并指定输出到控制台。

SQL注入：是通过操作输入的数据来修改事先定义好的SQL语句（拼接SQL语句），以达到执行代码对服务器进行攻击的方法。

#### 参数占位符

- #{...}：将会被替换为?，生成预编译SQL，会自动设置参数值——参数传递等情况都使用
- ${...}：直接将参数拼接在SQL语句中，存在SQL注入问题——对表名、列表进行动态设置时使用

**#{} 和 ${} 的区别是什么？**

- `${}`是 Properties 文件中的变量占位符，它可以用于标签属性值和 sql 内部，属于原样文本替换，可以替换任意内容，比如${driver}会被原样替换为`com.mysql.jdbc. Driver`。
  - 示例：`select * from users order by ${orderCols}` 。`orderCols`可以是 `name`、`name desc`、`name,sex asc`等，实现灵活的排序。
- `#{}`是 sql 的参数占位符，MyBatis 会将 sql 中的`#{}`替换为? 号，在 sql 执行前会使用 PreparedStatement 的参数设置方法，按序给 sql 的? 号占位符设置参数值，比如 ps.setInt(0, parameterValue)，`#{item.name}` 的取值方式为使用反射从参数对象中获取 item 对象的 name 属性值，相当于 `param.getItem().getName()`。

### 新增

`@Insert("sql语句")`	因为需要传入的参数过多，所以将对象作为参数传入

#### 主键返回

@Options(keyProperty="id", useGeneratedKeys=true) 会自动将生成的主键值，赋值给对象的 id 属性

### 更新

`@Update("sql语句")`	传入的参数为对象

ps. 别忘了 UpdateTime！

### 查询

`@Select("sql语句")`

#### 根据ID查询

**数据封装**

实体类属性名和数据库表查询返回的字段名一致，mybatis会自动封装，反之不会。

- 方案一：给字段起别名，让别名与实体类属性一致
- 方案二：通过 `@Results`，`@Result` 注解手动映射封装
- 方案三：在 properties 文件中开启 mybatis 的驼峰命名自动映射开关——下划线字段名会自动映射为驼峰命名

#### 条件查询

查询结果一般为多条数据，封装至类型为 `List<对象> list()`

**sql注入问题**

条件查询时，例如 `%${name}%`（%姓%）的语句形式生成的并非预编译SQL

解决：使用字符串拼接函数 concat('', #{...}, '')  —— `concat('%', #{...}, '%')`

## 动态SQL

### XML映射文件

- XML映射文件的名称与 Mapper 接口名称一致，并且将 XML映射文件和 Mapper 接口放在同一包下（同包同名）
- XML映射文件的 namespace 属性为 Mapper 接口全类名
- XML映射文件中 SQL 语句的 `id` 和 Mapper 接口中的 `方法名` 一致，并保持返回类型 `resultType `一致

使用 Mybatis 注解主要是来完成一些简单的增删改查功能，使之显得更加简洁；如果需要实现复杂的功能，最好使用 XML 来配置映射语句。

**MybatisX 插件，提高 Mybatis 的开发效率**

#### xml 映射文件中，除了常见的 select、insert、update、delete 标签之外，还有哪些标签？

还有很多其他的标签， `<resultMap>`、 `<parameterMap>`、 `<sql>`、 `<include>`、 `<selectKey>` ，加上动态 sql 的 9 个标签， `trim | where | set | foreach | if | choose | when | otherwise | bind` 等，其中 `<sql>` 为 sql 片段标签，通过 `<include>` 标签引入 sql 片段， `<selectKey>` 为不支持自增的主键生成策略标签。

### 动态SQL

MyBatis 动态 sql 可以让我们在 xml 映射文件内，以标签的形式编写动态 sql，完成逻辑判断和动态拼接 sql 的功能。其执行原理为，使用 OGNL 从 sql 参数对象中计算表达式的值，根据表达式的值动态拼接 sql，以此来完成动态 sql 的功能。

动态SQL就是根据用户的输入或者外部条件的变化而变化的 SQL 语句

MyBatis 提供了 9 种动态 sql 标签:

- `<if></if>`
- `<where></where>(trim,set)`
- `<choose></choose>（when, otherwise）`
- `<foreach></foreach>`
- `<bind/>`

### if

动态条件查询。用于判断条件是否成立。使用 test 属性进行条件判断，如果为 true 则拼接 SQL 。

```sql
<if test="name != null">
	name like concat('%', #{name}, '%')
</if>
```

多余 and 关键字问题：where 关键字改为 `<where></where>` 标签

动态更新：如果字段有传递值则更新，没有传递值则不更新。

### foreach

动态循环遍历。例如批量删除操作。

属性：

- collection：遍历的集合
- item：遍历的结果元素
- separator：分隔符
- open / close：遍历开始前 / 结束后拼接的 SQL 片段

```sql
<foreach collection="ids" item="id" seprator="," open="(" close=")">
	#{id}
</foreach>
```

### sql & include

`<sql id=""></sql>` ：定义复用的 SQL 片段

`<include refid=""/>` ：通过 refid 属性指定包含的 SQL 片段



## 常见面试题

### MyBatis 动态 sql 是做什么的？都有哪些动态 sql？能简述一下动态 sql 的执行原理不？

MyBatis 动态 sql 可以让我们在 xml 映射文件内，以标签的形式编写动态 sql，完成逻辑判断和动态拼接 sql 的功能。其执行原理为，使用 OGNL 从 sql 参数对象中计算表达式的值，根据表达式的值动态拼接 sql，以此来完成动态 sql 的功能。

MyBatis 提供了 9 种动态 sql 标签:

- `<if></if>`
- `<where></where>(trim,set)`
- `<choose></choose>（when, otherwise）`
- `<foreach></foreach>`
- `<bind/>`

关于 MyBatis 动态 SQL 的详细介绍，请看这篇文章：[Mybatis 系列全解（八）：Mybatis 的 9 大动态 SQL 标签你知道几个？](https://segmentfault.com/a/1190000039335704) 。

关于这些动态 SQL 的具体使用方法，请看这篇文章：[Mybatis【13】-- Mybatis 动态 sql 标签怎么使用？](https://cloud.tencent.com/developer/article/1943349)

### MyBatis 是如何将 sql 执行结果封装为目标对象并返回的？都有哪些映射形式？

第一种是使用 `<resultMap>` 标签，逐一定义列名和对象属性名之间的映射关系。第二种是使用 sql 列的别名功能，将列别名书写为对象属性名，比如 T_NAME AS NAME，对象属性名一般是 name，小写，但是列名不区分大小写，MyBatis 会忽略列名大小写，智能找到与之对应对象属性名，你甚至可以写成 T_NAME AS NaMe，MyBatis 一样可以正常工作。

有了列名与属性名的映射关系后，MyBatis 通过反射创建对象，同时使用反射给对象的属性逐一赋值并返回，那些找不到映射关系的属性，是无法完成赋值的。

### Dao 接口的工作原理是什么？Dao 接口里的方法，参数不同时，方法能重载吗？

最佳实践中，通常一个 xml 映射文件，都会写一个 Dao 接口与之对应。Dao 接口就是人们常说的 `Mapper` 接口，接口的全限名，就是映射文件中的 namespace 的值，接口的方法名，就是映射文件中 `MappedStatement` 的 id 值，接口方法内的参数，就是传递给 sql 的参数。 `Mapper` 接口是没有实现类的，当调用接口方法时，接口全限名+方法名拼接字符串作为 key 值，可唯一定位一个 `MappedStatement` ，举例：`com.mybatis3.mappers. StudentDao.findStudentById` ，可以唯一找到 namespace 为 `com.mybatis3.mappers. StudentDao` 下面 `id = findStudentById` 的 `MappedStatement` 。在 MyBatis 中，每一个 `<select>`、 `<insert>`、 `<update>`、 `<delete>` 标签，都会被解析为一个 `MappedStatement` 对象。

Dao 接口里的方法可以重载，但是 Mybatis 的 xml 里面的 ID 不允许重复。**Mybatis 的 Dao 接口可以有多个重载方法，但是多个接口对应的映射必须只有一个，否则启动会报错。**Dao 接口的工作原理是 JDK 动态代理，MyBatis 运行时会使用 JDK 动态代理为 Dao 接口生成代理 proxy 对象，代理对象 proxy 会拦截接口方法，转而执行 `MappedStatement` 所代表的 sql，然后将 sql 执行结果返回。

**补充**：

Dao 接口方法可以重载，但是需要满足以下条件：

1. 仅有一个无参方法和一个有参方法
2. 多个有参方法时，参数数量必须一致。且使用相同的 `@Param` ，或者使用 `param1` 这种

### MyBatis 是如何进行分页的？分页插件的原理是什么？

**(1)** MyBatis 使用 RowBounds 对象进行分页，它是针对 ResultSet 结果集执行的内存分页，而非物理分页；**(2)** 可以在 sql 内直接书写带有物理分页的参数来完成物理分页功能，**(3)** 也可以使用分页插件来完成物理分页。

分页插件的基本原理是使用 MyBatis 提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的 sql，然后重写 sql，根据 dialect 方言，添加对应的物理分页语句和物理分页参数。

举例：`select _ from student` ，拦截 sql 后重写为：`select t._ from （select \* from student）t limit 0，10`

### MyBatis 执行批量插入，能返回数据库主键列表吗？

能，JDBC 都能，MyBatis 当然也能。

### MyBatis 能执行一对一、一对多的关联查询吗？都有哪些实现方式，以及它们之间的区别

能，MyBatis 不仅可以执行一对一、一对多的关联查询，还可以执行多对一，多对多的关联查询，多对一查询，其实就是一对一查询，只需要把 `selectOne()` 修改为 `selectList()` 即可；多对多查询，其实就是一对多查询，只需要把 `selectOne()` 修改为 `selectList()` 即可。

关联对象查询，有两种实现方式，一种是单独发送一个 sql 去查询关联对象，赋给主对象，然后返回主对象。另一种是使用嵌套查询，嵌套查询的含义为使用 join 查询，一部分列是 A 对象的属性值，另外一部分列是关联对象 B 的属性值，好处是只发一个 sql 查询，就可以把主对象和其关联对象查出来。

那么问题来了，join 查询出来 100 条记录，如何确定主对象是 5 个，而不是 100 个？其去重复的原理是 `<resultMap>` 标签内的 `<id>` 子标签，指定了唯一确定一条记录的 id 列，MyBatis 根据 `<id>` 列值来完成 100 条记录的去重复功能， `<id>` 可以有多个，代表了联合主键的语意。

同样主对象的关联对象，也是根据这个原理去重复的，尽管一般情况下，只有主对象会有重复记录，关联对象一般不会重复。

### 简述 MyBatis 的插件运行原理，以及如何编写一个插件

### MyBatis 是否支持延迟加载？如果支持，它的实现原理是什么？

MyBatis 仅支持 association 关联对象和 collection 关联集合对象的延迟加载，association 指的就是一对一，collection 指的就是一对多查询。在 MyBatis 配置文件中，可以配置是否启用延迟加载 `lazyLoadingEnabled=true|false。`

它的原理是，使用 `CGLIB` 创建目标对象的代理对象，当调用目标方法时，进入拦截器方法，比如调用 `a.getB().getName()` ，拦截器 `invoke()` 方法发现 `a.getB()` 是 null 值，那么就会单独发送事先保存好的查询关联 B 对象的 sql，把 B 查询上来，然后调用 a.setB(b)，于是 a 的对象 b 属性就有值了，接着完成 `a.getB().getName()` 方法的调用。这就是延迟加载的基本原理。

当然了，不光是 MyBatis，几乎所有的包括 Hibernate，支持延迟加载的原理都是一样的。

### MyBatis 的 xml 映射文件中，不同的 xml 映射文件，id 是否可以重复？

不同的 xml 映射文件，如果配置了 namespace，那么 id 可以重复；如果没有配置 namespace，那么 id 不能重复；毕竟 namespace 不是必须的，只是最佳实践而已。

原因就是 namespace+id 是作为 `Map<String, MappedStatement>` 的 key 使用的，如果没有 namespace，就剩下 id，那么，id 重复会导致数据互相覆盖。有了 namespace，自然 id 就可以重复，namespace 不同，namespace+id 自然也就不同。

### MyBatis 中如何执行批处理？

使用 `BatchExecutor` 完成批处理。

### MyBatis 都有哪些 Executor 执行器？它们之间的区别是什么？

MyBatis 有三种基本的 `Executor` 执行器：

- **`SimpleExecutor`：** 每执行一次 update 或 select，就开启一个 Statement 对象，用完立刻关闭 Statement 对象。
- **`ReuseExecutor`：** 执行 update 或 select，以 sql 作为 key 查找 Statement 对象，存在就使用，不存在就创建，用完后，不关闭 Statement 对象，而是放置于 Map<String, Statement>内，供下一次使用。简言之，就是重复使用 Statement 对象。
- **`BatchExecutor`**：执行 update（没有 select，JDBC 批处理不支持 select），将所有 sql 都添加到批处理中（addBatch()），等待统一执行（executeBatch()），它缓存了多个 Statement 对象，每个 Statement 对象都是 addBatch()完毕后，等待逐一执行 executeBatch()批处理。与 JDBC 批处理相同。

作用范围：`Executor` 的这些特点，都严格限制在 SqlSession 生命周期范围内。

### MyBatis 中如何指定使用哪一种 Executor 执行器？

在 MyBatis 配置文件中，可以指定默认的 `ExecutorType` 执行器类型，也可以手动给 `DefaultSqlSessionFactory` 的创建 SqlSession 的方法传递 `ExecutorType` 类型参数。

### MyBatis 是否可以映射 Enum 枚举类？

MyBatis 可以映射枚举类，不单可以映射枚举类，MyBatis 可以映射任何对象到表的一列上。映射方式为自定义一个 `TypeHandler` ，实现 `TypeHandler` 的 `setParameter()` 和 `getResult()` 接口方法。 `TypeHandler` 有两个作用：

- 一是完成从 javaType 至 jdbcType 的转换；
- 二是完成 jdbcType 至 javaType 的转换，体现为 `setParameter()` 和 `getResult()` 两个方法，分别代表设置 sql 问号占位符参数和获取列查询结果。

### MyBatis 映射文件中，如果 A 标签通过 include 引用了 B 标签的内容，请问，B 标签能否定义在 A 标签的后面，还是说必须定义在 A 标签的前面？

虽然 MyBatis 解析 xml 映射文件是按照顺序解析的，但是，被引用的 B 标签依然可以定义在任何地方，MyBatis 都可以正确识别。

原理是，MyBatis 解析 A 标签，发现 A 标签引用了 B 标签，但是 B 标签尚未解析到，尚不存在，此时，MyBatis 会将 A 标签标记为未解析状态，然后继续解析余下的标签，包含 B 标签，待所有标签解析完毕，MyBatis 会重新解析那些被标记为未解析的标签，此时再解析 A 标签时，B 标签已经存在，A 标签也就可以正常解析完成了。

### 简述 MyBatis 的 xml 映射文件和 MyBatis 内部数据结构之间的映射关系？

MyBatis 将所有 xml 配置信息都封装到 All-In-One 重量级对象 Configuration 内部。在 xml 映射文件中， `<parameterMap>` 标签会被解析为 `ParameterMap` 对象，其每个子元素会被解析为 ParameterMapping 对象。 `<resultMap>` 标签会被解析为 `ResultMap` 对象，其每个子元素会被解析为 `ResultMapping` 对象。每一个 `<select>、<insert>、<update>、<delete>` 标签均会被解析为 `MappedStatement` 对象，标签内的 sql 会被解析为 BoundSql 对象。

### 为什么说 MyBatis 是半自动 ORM 映射工具？它与全自动的区别在哪里？

Hibernate 属于全自动 ORM 映射工具，使用 Hibernate 查询关联对象或者关联集合对象时，可以根据对象关系模型直接获取，所以它是全自动的。而 MyBatis 在查询关联对象或关联集合对象时，需要手动编写 sql 来完成，所以，称之为半自动 ORM 映射工具。



面试题看似都很简单，但是想要能正确回答上来，必定是研究过源码且深入的人，而不是仅会使用的人或者用的很熟的人。