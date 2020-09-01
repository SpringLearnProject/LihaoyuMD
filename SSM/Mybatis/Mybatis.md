### Mybatis

#### 一 ， XML配置

##### 文件示例

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="dbconfig.properties"></properties>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
				<property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://192.168.132.135:3306/Mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="123"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="BlogMapper.xml"/>
    </mappers>
</configuration>
```



##### 1，properties

```xml
<properties resource="dbconfig.properties"></properties>
```

> resource必须是全类名

###### 方法一 外部配置文件

- 可以使用外部配置文件`dbconfig.properties`替换

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://192.168.132.135:3306/Mybatis
jdbc.username=root
jdbc.password=123
```

然后将xml中的property替换为

```xml
<property name="driver" value="${jdbc.driver}"/>
<property name="url" value="${jdbc.url}"/>
<property name="username" value="${jdbc.username}"/>
<property name="password" value="${jdbc.password}"/>
```

> 就不要在代码中写账户密码，更加安全，节省代码量

###### 方法二 直接在xml中配置

```xml
<properties resource="全路径">
  <property name="username" value="dev_user"/>
  <property name="password" value="F2Fa3!33TYyg"/>
</properties>
```

> 同样的，在xml文件中use $ get value
>
> ```xml
> <dataSource type="POOLED">
>   <property name="driver" value="${driver}"/>
>   <property name="url" value="${url}"/>
>   <property name="username" value="${username}"/>
>   <property name="password" value="${password}"/>
> </dataSource>
> ```
>
> 

##### 2,setting

| 设置名                           | 描述                                                         | 有效值                                                       | 默认值                                                |
| :------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :---------------------------------------------------- |
| cacheEnabled                     | 全局地开启或关闭配置文件中的所有映射器已经配置的任何缓存。   | true \| false                                                | true                                                  |
| lazyLoadingEnabled               | 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置 `fetchType` 属性来覆盖该项的开关状态。 | true \| false                                                | false                                                 |
| aggressiveLazyLoading            | 当开启时，任何方法的调用都会加载该对象的所有属性。 否则，每个属性会按需加载（参考 `lazyLoadTriggerMethods`)。 | true \| false                                                | false （在 3.4.1 及之前的版本默认值为 true）          |
| multipleResultSetsEnabled        | 是否允许单一语句返回多结果集（需要驱动支持）。               | true \| false                                                | true                                                  |
| useColumnLabel                   | 使用列标签代替列名。不同的驱动在这方面会有不同的表现，具体可参考相关驱动文档或通过测试这两种不同的模式来观察所用驱动的结果。 | true \| false                                                | true                                                  |
| useGeneratedKeys                 | 允许 JDBC 支持自动生成主键，需要驱动支持。 如果设置为 true 则这个设置强制使用自动生成主键，尽管一些驱动不能支持但仍可正常工作（比如 Derby）。 | true \| false                                                | False                                                 |
| autoMappingBehavior              | 指定 MyBatis 应如何自动映射列到字段或属性。 NONE 表示取消自动映射；PARTIAL 只会自动映射没有定义嵌套结果集映射的结果集。 FULL 会自动映射任意复杂的结果集（无论是否嵌套）。 | NONE, PARTIAL, FULL                                          | PARTIAL                                               |
| autoMappingUnknownColumnBehavior | 指定发现自动映射目标未知列（或者未知属性类型）的行为。`NONE`: 不做任何反应`WARNING`: 输出提醒日志 (`'org.apache.ibatis.session.AutoMappingUnknownColumnBehavior'`的日志等级必须设置为 `WARN`)`FAILING`: 映射失败 (抛出 `SqlSessionException`) | NONE, WARNING, FAILING                                       | NONE                                                  |
| defaultExecutorType              | 配置默认的执行器。SIMPLE 就是普通的执行器；REUSE 执行器会重用预处理语句（prepared statements）； BATCH 执行器将重用语句并执行批量更新。 | SIMPLE REUSE BATCH                                           | SIMPLE                                                |
| defaultStatementTimeout          | 设置超时时间，它决定驱动等待数据库响应的秒数。               | 任意正整数                                                   | 未设置 (null)                                         |
| defaultFetchSize                 | 为驱动的结果集获取数量（fetchSize）设置一个提示值。此参数只可以在查询设置中被覆盖。 | 任意正整数                                                   | 未设置 (null)                                         |
| defaultResultSetType             | Specifies a scroll strategy when omit it per statement settings. (Since: 3.5.2) | FORWARD_ONLY \| SCROLL_SENSITIVE \| SCROLL_INSENSITIVE \| DEFAULT(same behavior with 'Not Set') | Not Set (null)                                        |
| safeRowBoundsEnabled             | 允许在嵌套语句中使用分页（RowBounds）。如果允许使用则设置为 false。 | true \| false                                                | False                                                 |
| safeResultHandlerEnabled         | 允许在嵌套语句中使用分页（ResultHandler）。如果允许使用则设置为 false。 | true \| false                                                | True                                                  |
| mapUnderscoreToCamelCase         | 是否开启自动驼峰命名规则（camel case）映射，即从经典数据库列名 A_COLUMN 到经典 Java 属性名 aColumn 的类似映射。 | true \| false                                                | False                                                 |
| localCacheScope                  | MyBatis 利用本地缓存机制（Local Cache）防止循环引用（circular references）和加速重复嵌套查询。 默认值为 SESSION，这种情况下会缓存一个会话中执行的所有查询。 若设置值为 STATEMENT，本地会话仅用在语句执行上，对相同 SqlSession 的不同调用将不会共享数据。 | SESSION \| STATEMENT                                         | SESSION                                               |
| jdbcTypeForNull                  | 当没有为参数提供特定的 JDBC 类型时，为空值指定 JDBC 类型。 某些驱动需要指定列的 JDBC 类型，多数情况直接用一般类型即可，比如 NULL、VARCHAR 或 OTHER。 | JdbcType 常量，常用值：NULL, VARCHAR 或 OTHER。              | OTHER                                                 |
| lazyLoadTriggerMethods           | 指定哪个对象的方法触发一次延迟加载。                         | 用逗号分隔的方法列表。                                       | equals,clone,hashCode,toString                        |
| defaultScriptingLanguage         | 指定动态 SQL 生成的默认语言。                                | 一个类型别名或完全限定类名。                                 | org.apache.ibatis.scripting.xmltags.XMLLanguageDriver |
| defaultEnumTypeHandler           | 指定 Enum 使用的默认 `TypeHandler` 。（新增于 3.4.5）        | 一个类型别名或完全限定类名。                                 | org.apache.ibatis.type.EnumTypeHandler                |
| callSettersOnNulls               | 指定当结果集中值为 null 的时候是否调用映射对象的 setter（map 对象时为 put）方法，这在依赖于 Map.keySet() 或 null 值初始化的时候比较有用。注意基本类型（int、boolean 等）是不能设置成 null 的。 | true \| false                                                | false                                                 |
| returnInstanceForEmptyRow        | 当返回行的所有列都是空时，MyBatis默认返回 `null`。 当开启这个设置时，MyBatis会返回一个空实例。 请注意，它也适用于嵌套的结果集 （如集合或关联）。（新增于 3.4.2） | true \| false                                                | false                                                 |
| logPrefix                        | 指定 MyBatis 增加到日志名称的前缀。                          | 任何字符串                                                   | 未设置                                                |
| logImpl                          | 指定 MyBatis 所用日志的具体实现，未指定时将自动查找。        | SLF4J \| LOG4J \| LOG4J2 \| JDK_LOGGING \| COMMONS_LOGGING \| STDOUT_LOGGING \| NO_LOGGING | 未设置                                                |
| proxyFactory                     | 指定 Mybatis 创建具有延迟加载能力的对象所用到的代理工具。    | CGLIB \| JAVASSIST                                           | JAVASSIST （MyBatis 3.3 以上）                        |
| vfsImpl                          | 指定 VFS 的实现                                              | 自定义 VFS 的实现的类全限定名，以逗号分隔。                  | 未设置                                                |
| useActualParamName               | 允许使用方法签名中的名称作为语句参数名称。 为了使用该特性，你的项目必须采用 Java 8 编译，并且加上 `-parameters` 选项。（新增于 3.4.1） | true \| false                                                | true                                                  |
| configurationFactory             | 指定一个提供 `Configuration` 实例的类。 这个被返回的 Configuration 实例用来加载被反序列化对象的延迟加载属性值。 这个类必须包含一个签名为`static Configuration getConfiguration()` 的方法。（新增于 3.2.3） | 类型别名或者全类名.                                          | 未设置                                                |

```xml
<settings>
  <setting name="cacheEnabled" value="true"/>
  <setting name="lazyLoadingEnabled" value="true"/>
  <setting name="multipleResultSetsEnabled" value="true"/>
  <setting name="useColumnLabel" value="true"/>
  <setting name="useGeneratedKeys" value="false"/>
  <setting name="autoMappingBehavior" value="PARTIAL"/>
  <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
  <setting name="defaultExecutorType" value="SIMPLE"/>
  <setting name="defaultStatementTimeout" value="25"/>
  <setting name="defaultFetchSize" value="100"/>
  <setting name="safeRowBoundsEnabled" value="false"/>
  <setting name="mapUnderscoreToCamelCase" value="false"/>
  <setting name="localCacheScope" value="SESSION"/>
  <setting name="jdbcTypeForNull" value="OTHER"/>
  <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
</settings>
```

##### 3,typeAliases

类型别名是为 Java 类型设置一个短的名字。 它只和 XML 配置有关，存在的意义仅在于用来减少类完全限定名的冗余.

```xml
<typeAliases>
  <typeAlias alias="Author" type="domain.blog.Author"/>
  <typeAlias alias="Blog" type="domain.blog.Blog"/>
  <typeAlias alias="Comment" type="domain.blog.Comment"/>
  <typeAlias alias="Post" type="domain.blog.Post"/>
  <typeAlias alias="Section" type="domain.blog.Section"/>
  <typeAlias alias="Tag" type="domain.blog.Tag"/>
</typeAliases>
the alise is designate(代称),type is Full category(全类名).
```

也可以指定一个包名，来实现批量的建立别称，但这些别称都是自动生成的。

```xml
<typeAliases>
  <package name="domain.blog"/>
</typeAliases>
```

##### 4,typeHandlers

无论是 MyBatis 在预处理语句（PreparedStatement）中设置一个参数时，还是从结果集中取出一个值时， 都会用类型处理器将获取的值以合适的方式转换成 Java 类型.



你也可以重写类型处理器或创建你自己的类型处理器来处理不支持的或非标准的类型。 具体做法为：实现 `org.apache.ibatis.type.TypeHandler` 接口， 或继承一个很便利的类 `org.apache.ibatis.type.BaseTypeHandler`， 然后可以选择性地将它映射到一个 JDBC 类型。比如：

```java
// ExampleTypeHandler.java
@MappedJdbcTypes(JdbcType.VARCHAR)
public class ExampleTypeHandler extends BaseTypeHandler<String> {

  @Override
  public void setNonNullParameter(PreparedStatement ps, int i, String parameter, JdbcType jdbcType) throws SQLException {
    ps.setString(i, parameter);
  }

  @Override
  public String getNullableResult(ResultSet rs, String columnName) throws SQLException {
    return rs.getString(columnName);
  }

  @Override
  public String getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
    return rs.getString(columnIndex);
  }

  @Override
  public String getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
    return cs.getString(columnIndex);
  }
}
```

corresponding`相应的`,xml config file is like this:

```xml
<!-- mybatis-config.xml -->
<typeHandlers>
  <typeHandler handler="org.mybatis.example.ExampleTypeHandler"/>
</typeHandlers>
```

##### 5,插件（plugins）

 MyBatis 允许你在已映射语句执行过程中的某一点进行拦截调用。默认情况下，MyBatis 允许使用插件来拦截的方法调用包括：

- Executor (update, query, flushStatements, commit, rollback, getTransaction, close, isClosed)
- ParameterHandler (getParameterObject, setParameters)
- ResultSetHandler (handleResultSets, handleOutputParameters)
- StatementHandler (prepare, parameterize, batch, update, query)

这些类中方法的细节可以通过查看每个方法的签名来发现，或者直接查看 MyBatis 发行包中的源代码。 如果你想做的不仅仅是监控方法的调用，那么你最好相当了解要重写的方法的行为。 因为如果在试图修改或重写已有方法的行为的时候，你很可能在破坏 MyBatis 的核心模块。 这些都是更低层的类和方法，所以使用插件的时候要特别当心。

通过 MyBatis 提供的强大机制，使用插件是非常简单的，只需实现 Interceptor 接口，并指定想要拦截的方法签名即可。

```java
// ExamplePlugin.java
@Intercepts({@Signature(
  type= Executor.class,
  method = "update",
  args = {MappedStatement.class,Object.class})})
public class ExamplePlugin implements Interceptor {
  private Properties properties = new Properties();
  public Object intercept(Invocation invocation) throws Throwable {
    // implement pre processing if need
    Object returnObject = invocation.proceed();
    // implement post processing if need
    return returnObject;
  }
  public void setProperties(Properties properties) {
    this.properties = properties;
  }
}
```

```xml
<!-- mybatis-config.xml -->
<plugins>
  <plugin interceptor="org.mybatis.example.ExamplePlugin">
    <property name="someProperty" value="100"/>
  </plugin>
</plugins>
```

##### 6.环境（environment）

可是连接多个数据库，

```xml
<environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
				<property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://192.168.132.135:3306/Mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="123"/>
            </dataSource>
        </environment>
```

> 1,transactionManage:事务管理器  `如果你正在使用 Spring + MyBatis，则没有必要配置事务管理器， 因为 Spring 模块会使用自带的管理器来覆盖前面的配置。` type=”[JDBC|MANAGED]” 也可以`自定义`，只需要TransactionFactory接口。
>
> 
>
> 2，dataSource :数据源 type=”[UNPOOLED|POOLED|JNDI]”
>
> 你可以通过实现接口 `org.apache.ibatis.datasource.DataSourceFactory` 来使用第三方数据源：

##### 7，数据库厂商标识（databaseIdProvider）

支持不同厂商的mysql数据库

```xml
<databaseIdProvider type="DB_VENDOR">
  <property name="SQL Server" value="sqlserver"/>
  <property name="DB2" value="db2"/>
  <property name="Oracle" value="oracle" />
</databaseIdProvider>
```

> 然后在mapper文件中，对sql语句进行设置，如下：
>
> ```xml
> <select id="getEmById" resultType="Day04072101.Employee"
>         databaseid="oracle">
>     select * from Tab_text01 where id = #{id}
>   </select>
> ```
>
> databaseid填写value值。

##### 8,Mapper映射

将SQL映射注册到全局配置中。

1. ```xml
   <!-- 使用相对于类路径的资源引用 -->
   <mappers>
     <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
     <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
     <mapper resource="org/mybatis/builder/PostMapper.xml"/>
   </mappers>
   ```

2. ```xml
   <!-- 使用完全限定资源定位符（URL） -->
   <mappers>
     <mapper url="file:///var/mappers/AuthorMapper.xml"/>
     <mapper url="file:///var/mappers/BlogMapper.xml"/>
     <mapper url="file:///var/mappers/PostMapper.xml"/>
   </mappers>
   ```

3. ```xml
   <!-- 使用映射器接口实现类的完全限定类名 -->
   <mappers>
     <mapper class="org.mybatis.builder.AuthorMapper"/>
     <mapper class="org.mybatis.builder.BlogMapper"/>
     <mapper class="org.mybatis.builder.PostMapper"/>
   </mappers>
   ```

4. ```xml
   <!-- 将包内的映射器接口实现全部注册为映射器,在同包下，类名相同 -->
   <mappers>
     <package name="org.mybatis.builder"/>
   </mappers>
   ```

   

#### 二 , SQlMapperXml config

> `除select以外都要写openSession.commit();`否则无法更新成功

##### 文件样例：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="Day04072101.EmployeeMapper">
<!--    namespace 要写全类名，不然会出错，映射注册表不被其他的类所知-->
	<select id="getEmById" resultType="Day04072101.Employee" >
    	select * from Tab_text01 where id = #{id}
	</select>
</mapper>
```

##### 1,select

```xml
<select id="selectPerson" parameterType="int" resultType="hashmap">
  SELECT * FROM PERSON WHERE ID = #{id}
</select>
```

>这个语句被称作 selectPerson，接受一个 int（或 Integer）类型的参数，并返回一个 HashMap 类型的对象，其中的键是列名，值便是结果行中的对应值。注意参数符号：`#{id}`.

select 元素允许你配置很多属性来配置每条语句的作用细节。

```xml
<select
  id="selectPerson"
  parameterType="int"
  parameterMap="deprecated"
  resultType="hashmap"
  resultMap="personResultMap"
  flushCache="false"
  useCache="true"
  timeout="10"
  fetchSize="256"
  statementType="PREPARED"
  resultSetType="FORWARD_ONLY">
```

| 属性            | 描述                                                         |
| :-------------- | :----------------------------------------------------------- |
| `id`            | 在命名空间中唯一的标识符，可以被用来引用这条语句。           |
| `parameterType` | 将会传入这条语句的参数类的完全限定名或别名。这个属性是可选的，因为 MyBatis 可以通过类型处理器（TypeHandler） 推断出具体传入语句的参数，默认值为未设置（unset）。 |
| parameterMap    | 这是引用外部 parameterMap 的已经被废弃的方法。请使用内联参数映射和 parameterType 属性。 |
| `resultType`    | 从这条语句中返回的期望类型的类的完全限定名或别名。 注意如果返回的是集合，那应该设置为集合包含的类型，而不是集合本身。可以使用 resultType 或 resultMap，但不能同时使用。 |
| `resultMap`     | 外部 resultMap 的命名引用。结果集的映射是 MyBatis 最强大的特性，如果你对其理解透彻，许多复杂映射的情形都能迎刃而解。可以使用 resultMap 或 resultType，但不能同时使用。 |
| `flushCache`    | 将其设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认值：false。 |
| `useCache`      | 将其设置为 true 后，将会导致本条语句的结果被二级缓存缓存起来，默认值：对 select 元素为 true。 |
| `timeout`       | 这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为未设置（unset）（依赖驱动）。 |
| `fetchSize`     | 这是一个给驱动的提示，尝试让驱动程序每次批量返回的结果行数和这个设置值相等。 默认值为未设置（unset）（依赖驱动）。 |
| `statementType` | STATEMENT，PREPARED 或 CALLABLE 中的一个。这会让 MyBatis 分别使用 Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。 |
| `resultSetType` | FORWARD_ONLY，SCROLL_SENSITIVE, SCROLL_INSENSITIVE 或 DEFAULT（等价于 unset） 中的一个，默认值为 unset （依赖驱动）。 |
| `databaseId`    | 如果配置了数据库厂商标识（databaseIdProvider），MyBatis 会加载所有的不带 databaseId 或匹配当前 databaseId 的语句；如果带或者不带的语句都有，则不带的会被忽略。 |
| `resultOrdered` | 这个设置仅针对嵌套结果 select 语句适用：如果为 true，就是假设包含了嵌套结果集或是分组，这样的话当返回一个主结果行的时候，就不会发生有对前面结果集的引用的情况。 这就使得在获取嵌套的结果集的时候不至于导致内存不够用。默认值：`false`。 |
| `resultSets`    | 这个设置仅对多结果集的情况适用。它将列出语句执行后返回的结果集并给每个结果集一个名称，名称是逗号分隔的。 |

##### 2,insert,update,delete

```xml
<insert
  id="insertAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  keyProperty=""
  keyColumn=""
  useGeneratedKeys=""
  timeout="20">

<update
  id="updateAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  timeout="20">

<delete
  id="deleteAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  timeout="20">
```

参数解释：

| 属性               | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| `id`               | 命名空间中的唯一标识符，可被用来代表这条语句。               |
| `parameterType`    | 将要传入语句的参数的完全限定类名或别名。这个属性是可选的，因为 MyBatis 可以通过类型处理器推断出具体传入语句的参数，默认值为未设置（unset）。 |
| `parameterMap`     | 这是引用外部 parameterMap 的已经被废弃的方法。请使用内联参数映射和 parameterType 属性。 |
| `flushCache`       | 将其设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认值：true（对于 insert、update 和 delete 语句）。 |
| `timeout`          | 这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为未设置（unset）（依赖驱动）。 |
| `statementType`    | STATEMENT，PREPARED 或 CALLABLE 的一个。这会让 MyBatis 分别使用 Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。 |
| `useGeneratedKeys` | （仅对 insert 和 update 有用）这会令 MyBatis 使用 JDBC 的 getGeneratedKeys 方法来取出由数据库内部生成的主键（比如：像 MySQL 和 SQL Server 这样的关系数据库管理系统的自动递增字段），默认值：false。 |
| `keyProperty`      | （仅对 insert 和 update 有用）唯一标记一个属性，MyBatis 会通过 getGeneratedKeys 的返回值或者通过 insert 语句的 selectKey 子元素设置它的键值，默认值：未设置（`unset`）。如果希望得到多个生成的列，也可以是逗号分隔的属性名称列表。 |
| `keyColumn`        | （仅对 insert 和 update 有用）通过生成的键值设置表中的列名，这个设置仅在某些数据库（像 PostgreSQL）是必须的，当主键列不是表中的第一列的时候需要设置。如果希望使用多个生成的列，也可以设置为逗号分隔的属性名称列表。 |
| `databaseId`       | 如果配置了数据库厂商标识（databaseIdProvider），MyBatis 会加载所有的不带 databaseId 或匹配当前 databaseId 的语句；如果带或者不带的语句都有，则不带的会被忽略。 |

for example:

```xml
<insert id="insertAuthor">
  insert into Author (id,username,password,email,bio)
  values (#{id},#{username},#{password},#{email},#{bio})
</insert>

<update id="updateAuthor">
  update Author set
    username = #{username},
    password = #{password},
    email = #{email},
    bio = #{bio}
  where id = #{id}
</update>

<delete id="deleteAuthor">
  delete from Author where id = #{id}
</delete>
```

> 在EmployeeMapper接口类中实现这些方法，然后才可以在映射XML中写增删改查sql。

```java
public void addEmp(Employee employee);
public void updateEmp(Employee employee);
public void deleteEmpbyID(Integer id);
```

> 在实例类中添加有参无参构造器方法，方便后续构建employee方法，来传入参数。

```java
//无参构造器 alt+ins constructor select none
    public Employee() {
        super();
    }
    //有参构造器 alt+ins constructor 全选
    public Employee(Integer id, String LAST_name, String gender, String email) {
        this.id = id;
        this.LAST_name = LAST_name;
        this.gender = gender;
        this.email = email;
    }
```



###### 增测试

```java
@Test
    public void textADD() throws IOException{
        SqlSessionFactory sqlSessionFactory = getsqlSessionFactory();
//        opensession(),是需要手动提交的，也有另一种带参数的可以自动提交。
        SqlSession openSession = sqlSessionFactory.openSession();
        try{
            EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
//          当使用参数构造器来定义Employee时，new后面不会主动提示有参构造器方法，但有参构造器是存在的，输入参数自动会转化为有参构造器方法
            Employee employee = new Employee(null, "sdjadijasoi", null, "dsadawh@qq.com");
            mapper.addEmp(employee);

//            手动提交
            openSession.commit();
        }finally {
            openSession.close();
        }

    }
```

![image-20200730105101251](C:\Users\lidxk\AppData\Roaming\Typora\typora-user-images\image-20200730105101251.png)

###### update测试

```java
@Test
    public void textupgrade() throws IOException{
        SqlSessionFactory sqlSessionFactory = getsqlSessionFactory();
        SqlSession openSession = sqlSessionFactory.openSession();
        try{
            EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
            Employee employee = new Employee(12, "hdasijhd", null, "dsadawa@qq.com");
            mapper.updateEmp(employee);
            openSession.commit();
        }finally {
            openSession.close();
        }
    }
```

![image-20200730104951326](C:\Users\lidxk\AppData\Roaming\Typora\typora-user-images\image-20200730104951326.png)

###### 删测试

```jav
@Test
    public void textdelete() throws IOException{
        SqlSessionFactory sqlSessionFactory = getsqlSessionFactory();
        SqlSession openSession = sqlSessionFactory.openSession();
        try{
            EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
            mapper.deleteEmpbyID(12);
            openSession.commit();
        }finally {
            openSession.close();
        }
    }
```



![image-20200730105254863](C:\Users\lidxk\AppData\Roaming\Typora\typora-user-images\image-20200730105254863.png)

##### 获取主键自增值

> useGeneratedKeys="true",As a parameter`参数` used in the sql mapper .
>
> 指定对应的主键属性：将主键获取以后，打包成java.bean的那个属性。

###### such as:

```xml
<insert id="addEmp" useGeneratedKeys="true">
        insert into Tab_text01(LAST_name,gender,email)
        values(#{LAST_name},#{gender},#{email})
</insert>
```

> mysql支持自增，但Orcale不支持。需要先查询下一个主键值，然后插入这一个主键值对应的一行值才可成果。
>
> ```xml
> <insert>
> 	<selectkey keyproperty=id,order="BEFORE" resultType="integer">
>         
>     </selectkey>
> </insert>
> ```
>
> 在insert标签中插入selectkey标签语句，查询下一个主键值。
>
> ![image-20200730150733793](C:\Users\lidxk\AppData\Roaming\Typora\typora-user-images\image-20200730150733793.png)
>
> resultType定义查询德数值德返回值的类型是Integer。

##### 3，参数处理

单个参数没有任何异常

###### 多个参数：pojo，map，TO

>异常
>
>```log
>Cause: org.apache.ibatis.binding.BindingException: Parameter 'id' not found. Available parameters are [arg1, arg0, param1, param2]
>```
>
>多个参数是封装成map，
>
>​	key param1.....paramN 或者参数的索引
>
>​	value 传入的参数
>
>#{}就是从map中取的值。
>
>```xml
><select id="getEmployeeDouble" resultType="Day04072101.Employee">
>    select * from Tab_text01 where id=#{param1} and LAST_name=#{param2}
></select>
>```
>
>将参数的value 改为 paramN （`数值从1开始`）或者 argN（`数值从0开始`），`必须要指定resultType或者resultMap`，才能正确查询。
>
>为了见名知意，将param1转化为sql里面的参数，使用命名参数：明确指定分装参数时map的
>
>​		key：@param1("id") 
>
>​		value:参数值
>
>```java
> public Employee getEmployeeDouble(@Param("id") Integer id , @Param("LAST_name") String LAST_name);
>
>```
>
>pojo：如果我们传入的多个参数正好是我们业务逻辑的数据模型，就直接传入POJO。
>
>#{属性名}，取出传入的POJO属性
>
>###### `map`
>
>：如果多个参数不是业务逻辑的数据类型，没有对应的POJO，我们可以传入map
>
>#{key}，取出map对应的值
>
>```java
>public Employee getEmpMap(Map<String ,Object> map);
>```
>
>```java
>@Test
>    public void text02() throws IOException {
>        //获取sqlSessionFactory对象
>        SqlSessionFactory sqlSessionFactory = getsqlSessionFactory();
>        //获取SqlSession对象
>        SqlSession openSession = sqlSessionFactory.openSession();
>        //获取接口的是实现类对象
>        try{
>            EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
>            //查询结果
>//            Employee fdsfsd = mapper.getEmployeeDouble(322,"sdjadijasoi");
>            Map<String ,Object> map= new HashMap<String, Object>();
>            map.put("id",322);
>            map.put("LAST_name","sdjadijasoi");
>            Employee empMap = mapper.getEmpMap(map);
>
>            System.out.println(empMap);
>        }finally {
>            openSession.close();
>        }
>    }
>```
>
>如果参数不在业务模型中，又要经常使用，推荐编写一个TO（Transfer Object）数据传输对象。
>
>page{
>
>​	int index；//开始索引
>
>​	int size； //每页的大小
>
>}
>
>`特别注意`：如果时collection（list，set）类型，或者数组。把传入的list或者数组封装入map中。key Collection（collection），如果时list可以直接使用key（list），数组Key（array）
>
>```java
>public Employee getEmpId(List<integer> ids);
>#取值，取出第一个ID的值。
>    #{list[0]}
>```
>
>

###### 注意事项：

>单个参数：mybatis不会做特殊处理，
>	#{参数名/任意名}：取出参数值。
>	
>多个参数：mybatis会做特殊处理。
>	多个参数会被封装成 一个map，
>		key：param1...paramN,或者参数的索引也可以
>		value：传入的参数值
>	#{}就是从map中获取指定的key的值；
>	
>
>	异常：
>	org.apache.ibatis.binding.BindingException: 
>	Parameter 'id' not found. 
>	Available parameters are [1, 0, param1, param2]
>	操作：
>		方法：public Employee getEmpByIdAndLastName(Integer id,String lastName);
>		取值：#{id},#{lastName}
>
>【命名参数】：明确指定封装参数时map的key；@Param("id")
>	多个参数会被封装成 一个map，
>		key：使用@Param注解指定的值
>		value：参数值
>	#{指定的key}取出对应的参数值
>
>
>POJO：
>如果多个参数正好是我们业务逻辑的数据模型，我们就可以直接传入pojo；
>	#{属性名}：取出传入的pojo的属性值	
>
>Map：
>如果多个参数不是业务模型中的数据，没有对应的pojo，不经常使用，为了方便，我们也可以传入map
>	#{key}：取出map中对应的值
>
>TO：
>如果多个参数不是业务模型中的数据，但是经常要使用，推荐来编写一个TO（Transfer Object）数据传输对象
>Page{
>	int index;
>	int size;
>}
>
>========================思考================================	
>public Employee getEmp(@Param("id")Integer id,String lastName);
>	取值：id==>#{id/param1}   lastName==>#{param2}
>
>public Employee getEmp(Integer id,@Param("e")Employee emp);
>	取值：id==>#{param1}    lastName===>#{param2.lastName/e.lastName}
>
>##特别注意：如果是Collection（List、Set）类型或者是数组，
>		 也会特殊处理。也是把传入的list或者数组封装在map中。
>			key：Collection（collection）,如果是List还可以使用这个key(list)
>				数组(array)
>public Employee getEmpById(List<Integer> ids);
>	取值：取出第一个id的值：   #{list[0]}
>	
>========================结合源码，mybatis怎么处理参数==========================
>总结：参数多时会封装map，为了不混乱，我们可以使用@Param来指定封装时使用的key；
>#{key}就可以取出map中的值；
>
>(@Param("id")Integer id,@Param("lastName")String lastName);
>ParamNameResolver解析参数封装map的；
>//1、names：{0=id, 1=lastName}；构造器的时候就确定好了
>
>	确定流程：
>	1.获取每个标了param注解的参数的@Param的值：id，lastName；  赋值给name;
>	2.每次解析一个参数给map中保存信息：（key：参数索引，value：name的值）
>		name的值：
>			标注了param注解：注解的值
>			没有标注：
>				1.全局配置：useActualParamName（jdk1.8）：name=参数名
>				2.name=map.size()；相当于当前元素的索引
>	{0=id, 1=lastName,2=2}
>
>
>args【1，"Tom",'hello'】:
>
>public Object getNamedParams(Object[] args) {
>    final int paramCount = names.size();
>    //1、参数为null直接返回
>    if (args == null || paramCount == 0) {
>      return null;
>     
>    //2、如果只有一个元素，并且没有Param注解；args[0]：单个参数直接返回
>    } else if (!hasParamAnnotation && paramCount == 1) {
>      return args[names.firstKey()];
>     
>    //3、多个元素或者有Param标注
>    } else {
>      final Map<String, Object> param = new ParamMap<Object>();
>      int i = 0;
>     
>      //4、遍历names集合；{0=id, 1=lastName,2=2}
>      for (Map.Entry<Integer, String> entry : names.entrySet()) {
>     
>      	//names集合的value作为key;  names集合的key又作为取值的参考args[0]:args【1，"Tom"】:
>      	//eg:{id=args[0]:1,lastName=args[1]:Tom,2=args[2]}
>        param.put(entry.getValue(), args[entry.getKey()]);
>
>
>​        
>        // add generic param names (param1, param2, ...)param
>        //额外的将每一个参数也保存到map中，使用新的key：param1...paramN
>        //效果：有Param注解可以#{指定的key}，或者#{param1}
>        final String genericParamName = GENERIC_NAME_PREFIX + String.valueOf(i + 1);
>        // ensure not to overwrite parameter named with @Param
>        if (!names.containsValue(genericParamName)) {
>     ​     param.put(genericParamName, args[entry.getKey()]);
>        }
>        i++;
>      }
>      return param;
>    }
>  }
>}
>===========================参数值的获取======================================
>#{}：可以获取map中的值或者pojo对象属性的值；
>${}：可以获取map中的值或者pojo对象属性的值；
>
>
>select * from tbl_employee where id=${id} and last_name=#{lastName}
>Preparing: select * from tbl_employee where id=2 and last_name=?
>	区别：
>		#{}:是以预编译的形式，将参数设置到sql语句中；PreparedStatement；防止sql注入
>		${}:取出的值直接拼装在sql语句中；会有安全问题；
>		大多情况下，我们去参数的值都应该去使用#{}；
>		
>		原生jdbc不支持占位符的地方我们就可以使用${}进行取值
>		比如分表、排序。。。；按照年份分表拆分
>			select * from ${year}_salary where xxx;
>			select * from tbl_employee order by ${f_name} ${order}
>
>#{}:更丰富的用法：
>	规定参数的一些规则：
>	javaType、 jdbcType、 mode（存储过程）、 numericScale、
>	resultMap、 typeHandler、 jdbcTypeName、 expression（未来准备支持的功能）；
>
>```scala
>jdbcType通常需要在某种特定的条件下被设置：
>	在我们数据为null的时候，有些数据库可能不能识别mybatis对null的默认处理。比如Oracle（报错）；
>	
>	JdbcType OTHER：无效的类型；因为mybatis对所有的null都映射的是原生Jdbc的OTHER类型，oracle不能正确处理;
>	
>	由于全局配置中：jdbcTypeForNull=OTHER；oracle不支持；两种办法
>	1、#{email,jdbcType=OTHER};
>	2、jdbcTypeForNull=NULL
>		<setting name="jdbcTypeForNull" value="NULL"/>
>```
>
>

##### 4，resultMap（自定义javaBean封装规则）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="Day04072101.EmployeeMapperPlus">
<!--    自定义某个javaBean封装规则-->
<!--    id：唯一指定方便引用-->
    <resultMap id="MyEmp" type="Day04072101.Employee">
<!--     指定主键列的封装规则   -->
<!--     column:指定那一列    -->
<!--     property:指定对应的javaBean属性   -->
        <id column="id" property="id"/>
<!--        定义普通列的封装规则-->
        <result column="LAST_name" property="LAST_name"/>
<!--        其他不指定列会自定封装，我们只要写resultMap，就把全部属性都写上-->
        <result column="gender" property="gender"/>
        <result column="email" property="email"/>
    </resultMap>
    <select id="getEmpid" resultMap="MyEmp">
        select * from Tab_text01 where id=#{id};
    </select>
```

#### 三 动态sql

##### if

```xml
<select id="findActiveBlogWithTitleLike"
     resultType="Blog">
  SELECT * FROM BLOG
  WHERE state = ‘ACTIVE’
  <if test="title != null">
    AND title like #{title}
  </if>
</select>
```

两种方式：一种是将1==1 加在where关键词后面，另一种是使用where属性代替where关键词

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="Day04072101.DynamicEmployee">
<!--    if-->
<!--    <select id="getEmpByConditionIf" resultType="Day04072101.Employee">-->
<!--        select * from Tab_text01 where id=#{id}-->
<!--        <if test="LAST_name != null">-->
<!--            and LAST_name like #{LAST_name}-->
<!--        </if>-->
<!--        <if test="gender != null">-->
<!--            and gender=#{gender}-->
<!--        </if>-->
<!--        <if test="email != null">-->
<!--            and email =#{email}-->
<!--        </if>-->
<!--    </select>-->
    <select id="getEmpByConditionIf" resultType="Day04072101.Employee">
        select * from Tab_text01 
        <where>
            <if test="LAST_name != null">
                and LAST_name like #{LAST_name}
            </if>
            <if test="gender != null">
                and gender=#{gender}
            </if>
            <if test="email != null">
                and email =#{email}
            </if>
        </where>
    </select>
</mapper>
```

> where只会代替第一个and 或者 or。`不可把and放在sql语句结尾`。

##### 字符串截取

```xml
<select id="getEmpsByConditionTrim" resultType="com.atguigu.mybatis.bean.Employee">
   select * from tbl_employee
   <!-- 后面多出的and或者or where标签不能解决 
   prefix="":前缀：trim标签体中是整个字符串拼串 后的结果。
         prefix给拼串后的整个字符串加一个前缀 
   prefixOverrides="":
         前缀覆盖： 去掉整个字符串前面多余的字符
   suffix="":后缀
         suffix给拼串后的整个字符串加一个后缀 
   suffixOverrides=""
         后缀覆盖：去掉整个字符串后面多余的字符
         
   -->
   <!-- 自定义字符串的截取规则 -->
   <trim prefix="where" suffixOverrides="and">
      <if test="id!=null">
      id=#{id} and
   </if>
   <if test="lastName!=null &amp;&amp; lastName!=&quot;&quot;">
      last_name like #{lastName} and
   </if>
   <if test="email!=null and email.trim()!=&quot;&quot;">
      email=#{email} and
   </if> 
   <!-- ognl会进行字符串与数字的转换判断  "0"==0 -->
   <if test="gender==0 or gender==1">
      gender=#{gender}
   </if>
 </trim>
</select>
```

> 当其中有几个if确实为null时，会在结尾多出and，需要去掉。并在前端添加where关键词。

##### chose

```xml
<!-- public List<Employee> getEmpsByConditionChoose(Employee employee); -->
<select id="getEmpsByConditionChoose" resultType="com.atguigu.mybatis.bean.Employee">
   select * from tbl_employee 
   <where>
      <!-- 如果带了id就用id查，如果带了lastName就用lastName查;只会进入其中一个 -->
      <choose>
         <when test="id!=null">
            id=#{id}
         </when>
         <when test="lastName!=null">
            last_name like #{lastName}
         </when>
         <when test="email!=null">
            email = #{email}
         </when>
         <otherwise>
            gender = 0
         </otherwise>
      </choose>
   </where>
</select>
```

##### set

更新字段（有那些字段就改那些字段）

```xml
<!--public void updateEmp(Employee employee);  -->
    <update id="updateEmp">
      <!-- Set标签的使用 -->
      update tbl_employee 
      <set>
         <if test="lastName!=null">
            last_name=#{lastName},
         </if>
         <if test="email!=null">
            email=#{email},
         </if>
         <if test="gender!=null">
            gender=#{gender}
         </if>
      </set>
      where id=#{id} 
<!--      
      Trim：更新拼串
      update tbl_employee 
      <trim prefix="set" suffixOverrides=",">
         <if test="lastName!=null">
            last_name=#{lastName},
         </if>
         <if test="email!=null">
            email=#{email},
         </if>
         <if test="gender!=null">
            gender=#{gender}
         </if>
      </trim>
      where id=#{id}  -->
    </update>
```

##### foreach

```java
public List<Employee> getEmpsByConditionForeach(@Param("ids")List<Integer> ids);
```

```xml
<!--public List<Employee> getEmpsByConditionForeach(List<Integer> ids);  -->
<select id="getEmpsByConditionForeach" resultType="com.atguigu.mybatis.bean.Employee">
   select * from tbl_employee
   <!--
      collection：指定要遍历的集合：
         list类型的参数会特殊处理封装在map中，map的key就叫list
      item：将当前遍历出的元素赋值给指定的变量
      separator:每个元素之间的分隔符
      open：遍历出所有结果拼接一个开始的字符
      close:遍历出所有结果拼接一个结束的字符
      index:索引。遍历list的时候是index就是索引，item就是当前值
                  遍历map的时候index表示的就是map的key，item就是map的值
      
      #{变量名}就能取出变量的值也就是当前遍历出的元素
     -->
   <foreach collection="ids" item="item_id" separator=","
      open="where id in(" close=")">
      #{item_id}
   </foreach>
</select>
```

##### 批量保存

> 两种方式：
>
> 1. 循环获得emp的各个值：(#{emp.lastName},#{emp.email},#{emp.gender},#{emp.dept.id})
> 2. 循环输入sql语句：
>
> ```xml
> <foreach collection="emps" item="emp" separator=";">
>       insert into tbl_employee(last_name,email,gender,d_id)
>       values(#{emp.lastName},#{emp.email},#{emp.gender},#{emp.dept.id})
>    </foreach>
> ```
>
> 

```xml
<!-- 批量保存 -->
 <!--public void addEmps(@Param("emps")List<Employee> emps);  -->
 <!--MySQL下批量保存：可以foreach遍历   mysql支持values(),(),()语法-->
<insert id="addEmps">
   insert into tbl_employee(
      <include refid="insertColumn"></include>
   ) 
   values
   <foreach collection="emps" item="emp" separator=",">
      (#{emp.lastName},#{emp.email},#{emp.gender},#{emp.dept.id})
   </foreach>
 </insert><!--   -->
 
 <!-- 这种方式需要数据库连接属性allowMultiQueries=true；
   这种分号分隔多个sql可以用于其他的批量操作（删除，修改） -->
 <!-- <insert id="addEmps">
   <foreach collection="emps" item="emp" separator=";">
      insert into tbl_employee(last_name,email,gender,d_id)
      values(#{emp.lastName},#{emp.email},#{emp.gender},#{emp.dept.id})
   </foreach>
 </insert> -->
 
 <!-- Oracle数据库批量保存： 
   Oracle不支持values(),(),()
   Oracle支持的批量方式
   1、多个insert放在begin - end里面
      begin
          insert into employees(employee_id,last_name,email) 
          values(employees_seq.nextval,'test_001','test_001@atguigu.com');
          insert into employees(employee_id,last_name,email) 
          values(employees_seq.nextval,'test_002','test_002@atguigu.com');
      end;
   2、利用中间表：
      insert into employees(employee_id,last_name,email)
          select employees_seq.nextval,lastName,email from(
                 select 'test_a_01' lastName,'test_a_e01' email from dual
                 union
                 select 'test_a_02' lastName,'test_a_e02' email from dual
                 union
                 select 'test_a_03' lastName,'test_a_e03' email from dual
          )   
 -->
 <insert id="addEmps" databaseId="oracle">
   <!-- oracle第一种批量方式 -->
   <!-- <foreach collection="emps" item="emp" open="begin" close="end;">
      insert into employees(employee_id,last_name,email) 
          values(employees_seq.nextval,#{emp.lastName},#{emp.email});
   </foreach> -->
   
   <!-- oracle第二种批量方式  -->
   insert into employees(
      <!-- 引用外部定义的sql -->
      <include refid="insertColumn">
         <property name="testColomn" value="abc"/>
      </include>
   )
         <foreach collection="emps" item="emp" separator="union"
            open="select employees_seq.nextval,lastName,email from("
            close=")">
            select #{emp.lastName} lastName,#{emp.email} email from dual
         </foreach>
 </insert>
 
 
```

##### 内置参数

> ```xml
> <!-- 两个内置参数：
>    不只是方法传递过来的参数可以被用来判断，取值。。。
>    mybatis默认还有两个内置参数：
>    _parameter:代表整个参数
>       单个参数：_parameter就是这个参数
>       多个参数：参数会被封装为一个map；_parameter就是代表这个map
>    
>    _databaseId:如果配置了databaseIdProvider标签。
>       _databaseId就是代表当前数据库的别名oracle
>   -->
> ```
>
> 

##### 动态sql绑定

> ```xml
> <bind></bind>
> <-- 抽取固定的sql语句，方便后续重复使用-->
> <sql id = “insertConslect”>
> 	last_name,grander,email    
> </sql>
> include引用外部sql。    
> <insert>
> 	<include refid="insertConslect"></include>
> </insert>    
> ```
>
> 

##### 缓存

sqlsession级别的缓存，（一个sqlsession有一个一级缓存，不可改变）在一个session内的查询进行缓存，在一个session内重复的相同查询，不需要查询，会调用缓存中已有结果，而不实在的进行查询。

| `<cache>`       | 为给定的命名空间（比如类）配置缓存。属性有：`implemetation`, `eviction`, `flushInterval`, `size`, `readWrite`, `blocking` 和`properties`。 |
| --------------- | ------------------------------------------------------------ |
| `<eviction`     | 缓存的回收策略：LRU——最近最少被使用，一处最长时间不被使用的对象；FIFO——先进先出；SOFT——软引用，移除基于垃圾回收器状态和软引用规则的对象。WEAK——弱引用，更积极的基于垃圾回收器状态和软引用规则的对象，默认是LRU。 |
| `flushInterval` | 缓存刷新时间间隔。                                           |
| `readWrite`     | readonly：true or False                                      |
| 自定义cache     | 实现cache接口                                                |

```java
/**
 * 两级缓存：
 * 一级缓存：（本地缓存）：sqlSession级别的缓存。一级缓存是一直开启的；SqlSession级别的一个Map
 *        与数据库同一次会话期间查询到的数据会放在本地缓存中。
 *        以后如果需要获取相同的数据，直接从缓存中拿，没必要再去查询数据库；
 * 
 *        一级缓存失效情况（没有使用到当前一级缓存的情况，效果就是，还需要再向数据库发出查询）：
 *        1、sqlSession不同。
 *        2、sqlSession相同，查询条件不同.(当前一级缓存中还没有这个数据)
 *        3、sqlSession相同，两次查询之间执行了增删改操作(这次增删改可能对当前数据有影响)
 *        4、sqlSession相同，手动清除了一级缓存（缓存清空）
 * 
 * 二级缓存：（全局缓存）：基于namespace级别的缓存：一个namespace对应一个二级缓存：
 *        工作机制：
 *        1、一个会话，查询一条数据，这个数据就会被放在当前会话的一级缓存中；
 *        2、如果会话关闭；一级缓存中的数据会被保存到二级缓存中；新的会话查询信息，就可以参照二级缓存中的内容；
 *        3、sqlSession===EmployeeMapper==>Employee
 *                    DepartmentMapper===>Department
 *           不同namespace查出的数据会放在自己对应的缓存中（map）
 *           效果：数据会从二级缓存中获取
 *              查出的数据都会被默认先放在一级缓存中。
 *              只有会话提交或者关闭以后，一级缓存中的数据才会转移到二级缓存中
 *        使用：
 *           1）、开启全局二级缓存配置：<setting name="cacheEnabled" value="true"/>
 *           2）、去mapper.xml中配置使用二级缓存：
 *              <cache></cache>
 *           3）、我们的POJO需要实现序列化接口 importment Serializable
 *     
 * 和缓存有关的设置/属性：
 *           1）、cacheEnabled=true：false：关闭缓存（二级缓存关闭）(一级缓存一直可用的)
 *           2）、每个select标签都有useCache="true"：
 *                 false：不使用缓存（一级缓存依然使用，二级缓存不使用）
 *           3）、【每个增删改标签的：flushCache="true"：（一级二级都会清除）】
 *                 增删改执行完成后就会清楚缓存；
 *                 测试：flushCache="true"：一级缓存就清空了；二级也会被清除；
 *                 查询标签：flushCache="false"：
 *                    如果flushCache=true;每次查询之后都会清空缓存；缓存是没有被使用的；
 *           4）、sqlSession.clearCache();只是清楚当前session的一级缓存；
 *           5）、localCacheScope：本地缓存作用域：（一级缓存SESSION）；当前会话的所有数据保存在会话缓存中；
 *                          STATEMENT：可以禁用一级缓存；       
 *              
 *第三方缓存整合：
 *    1）、导入第三方缓存包即可；
 *    2）、导入与第三方缓存整合的适配包；官方有；
 *    3）、mapper.xml中使用自定义缓存
 *    <cache type="org.mybatis.caches.ehcache.EhcacheCache"></cache>
 *
 * @throws IOException 
 * 
```

###### 

#### 整合第三方cache——ehcache框架使用

mavendao导入ehcache依赖包

```xml
<!-- https://mvnrepository.com/artifact/net.sf.ehcache/ehcache -->
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache</artifactId>
    <version>2.10.6</version>
</dependency>

```

#### mybatis整合spring

```xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis-spring</artifactId>
  <version>2.0.5</version>
</dependency>
```



#### SSM框架整合

> maven依赖配置:尽量全面，不然会有配置文件错误
>
> ```xml
> <?xml version="1.0" encoding="UTF-8"?>
> <project xmlns="http://maven.apache.org/POM/4.0.0"
>          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
>          xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
>     <modelVersion>4.0.0</modelVersion>
> 
>     <groupId>SSMtextRe</groupId>
>     <artifactId>SSMtextRe</artifactId>
>     <version>1.0-SNAPSHOT</version>
> <dependencies>
>     <dependency>
>         <groupId>org.springframework</groupId>
>         <artifactId>spring-webmvc</artifactId>
>         <version>5.2.7.RELEASE</version>
>     </dependency>
>     <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
>     <dependency>
>         <groupId>org.springframework</groupId>
>         <artifactId>spring-context</artifactId>
>         <version>5.2.7.RELEASE</version>
>     </dependency>
>     <!-- https://mvnrepository.com/artifact/org.springframework/spring-tx -->
>     <dependency>
>         <groupId>org.springframework</groupId>
>         <artifactId>spring-tx</artifactId>
>         <version>5.2.7.RELEASE</version>
>     </dependency>
>     <!-- https://mvnrepository.com/artifact/org.springframework/spring-aspects -->
>     <dependency>
>         <groupId>org.springframework</groupId>
>         <artifactId>spring-aspects</artifactId>
>         <version>5.2.7.RELEASE</version>
>     </dependency>
>     <!-- https://mvnrepository.com/artifact/org.springframework/spring-orm -->
>     <dependency>
>         <groupId>org.springframework</groupId>
>         <artifactId>spring-orm</artifactId>
>         <version>5.2.7.RELEASE</version>
>     </dependency>
>     <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
>     <dependency>
>         <groupId>org.springframework</groupId>
>         <artifactId>spring-jdbc</artifactId>
>         <version>5.2.7.RELEASE</version>
>     </dependency>
>     <!-- https://mvnrepository.com/artifact/com.mchange/c3p0 -->
>     <dependency>
>         <groupId>com.mchange</groupId>
>         <artifactId>c3p0</artifactId>
>         <version>0.9.5.5</version>
>     </dependency>
>     <!-- https://mvnrepository.com/artifact/commons-logging/commons-logging -->
>     <dependency>
>         <groupId>commons-logging</groupId>
>         <artifactId>commons-logging</artifactId>
>         <version>1.2</version>
>     </dependency>
>     <!-- https://mvnrepository.com/artifact/org.apache.taglibs/taglibs-standard-impl -->
>     <dependency>
>         <groupId>org.apache.taglibs</groupId>
>         <artifactId>taglibs-standard-impl</artifactId>
>         <version>1.2.5</version>
>     </dependency>
>     <!-- https://mvnrepository.com/artifact/org.apache.taglibs/taglibs-standard-spec -->
>     <dependency>
>         <groupId>org.apache.taglibs</groupId>
>         <artifactId>taglibs-standard-spec</artifactId>
>         <version>1.2.5</version>
>     </dependency>
>     <!-- https://mvnrepository.com/artifact/net.sourceforge.cglib/com.springsource.net.sf.cglib -->
>     <dependency>
>         <groupId>net.sourceforge.cglib</groupId>
>         <artifactId>com.springsource.net.sf.cglib</artifactId>
>         <version>2.2.0</version>
>     </dependency>
>     <!-- https://mvnrepository.com/artifact/org.aopalliance/com.springsource.org.aopalliance -->
>     <dependency>
>         <groupId>org.aopalliance</groupId>
>         <artifactId>com.springsource.org.aopalliance</artifactId>
>         <version>1.0.0</version>
>     </dependency>
>     <!-- https://mvnrepository.com/artifact/org.aspectj/com.springsource.org.aspectj.weaver -->
>     <dependency>
>         <groupId>org.aspectj</groupId>
>         <artifactId>com.springsource.org.aspectj.weaver</artifactId>
>         <version>1.6.4.RELEASE</version>
>     </dependency>
> 
>     <dependency>
>         <groupId>org.mybatis</groupId>
>         <artifactId>mybatis</artifactId>
>         <version>3.5.5</version>
>     </dependency>
>     <dependency>
>         <groupId>mysql</groupId>
>         <artifactId>mysql-connector-java</artifactId>
>         <version>8.0.20</version>
>     </dependency>
>     <dependency>
>         <groupId>junit</groupId>
>         <artifactId>junit</artifactId>
>         <version>4.12</version>
>     </dependency>
>     <dependency>
>         <groupId>org.apache.logging.log4j</groupId>
>         <artifactId>log4j-api</artifactId>
>         <version>2.13.3</version>
>     </dependency>
>     <dependency>
>         <groupId>org.apache.logging.log4j</groupId>
>         <artifactId>log4j-core</artifactId>
>         <version>2.13.3</version>
>     </dependency>
>     <dependency>
>         <groupId>org.mybatis</groupId>
>         <artifactId>mybatis-spring</artifactId>
>         <version>2.0.5</version>
>     </dependency>
> </dependencies>
> 
> </project>
> ```
>
> 



