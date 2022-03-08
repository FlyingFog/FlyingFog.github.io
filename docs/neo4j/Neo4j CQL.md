# 2 Neo4j CQL

## CQL简介

CQL代表Cypher查询语言。 像Oracle数据库具有查询语言SQL，Neo4j具有CQL作为查询语言。



**Neo4j CQL **

- 它是Neo4j图形数据库的查询语言。
- 它是一种声明性模式匹配语言
- 它遵循SQL语法。
- 它的语法是非常简单且人性化、可读的格式。

- Neo4j CQL 已命令来执行数据库操作。
- Neo4j CQL 支持多个子句像在哪里，顺序等，以非常简单的方式编写非常复杂的查询。
- NNeo4j CQL 支持一些功能，如字符串，Aggregation.In 加入他们，它还支持一些关系功能。

### Neo4j CQL命令/条款

常用的Neo4j CQL命令/条款如下：

| S.No. | CQL命令/条      | 用法                         |
| ----- | --------------- | ---------------------------- |
| 1。   | CREATE 创建     | 创建节点，关系和属性         |
| 2。   | MATCH 匹配      | 检索有关节点，关系和属性数据 |
| 3。   | RETURN 返回     | 返回查询结果                 |
| 4。   | WHERE 哪里      | 提供条件过滤检索数据         |
| 5。   | DELETE 删除     | 删除节点和关系               |
| 6。   | REMOVE 移除     | 删除节点和关系的属性         |
| 7。   | ORDER BY以…排序 | 排序检索数据                 |
| 8。   | SET 组          | 添加或更新标签               |

### Neo4j CQL 函数

以下是常用的Neo4j CQL函数：

| S.No. | 定制列表功能      | 用法                                             |
| ----- | ----------------- | ------------------------------------------------ |
| 1。   | String 字符串     | 它们用于使用String字面量。                       |
| 2。   | Aggregation 聚合  | 它们用于对CQL查询结果执行一些聚合操作。          |
| 3。   | Relationship 关系 | 他们用于获取关系的细节，如startnode，endnode等。 |

我们将在后面的章节中详细讨论所有Neo4j CQL命令，子句和函数语法，用法和示例。

### Neo4j CQL数据类型

这些数据类型与Java语言类似。 它们用于定义节点或关系的属性
Neo4j CQL支持以下数据类型：

| S.No. | CQL数据类型 | 用法                            |
| ----- | ----------- | ------------------------------- |
| 1.    | boolean     | 用于表示布尔文字：true，false。 |
| 2.    | byte        | 用于表示8位整数。               |
| 3.    | short       | 用于表示16位整数。              |
| 4.    | int         | 用于表示32位整数。              |
| 5.    | long        | 用于表示64位整数。              |
| 6.    | float       | I用于表示32位浮点数。           |
| 7.    | double      | 用于表示64位浮点数。            |
| 8.    | char        | 用于表示16位字符。              |
| 9.    | String      | 用于表示字符串。                |



### 2.1 CREATE

Neo4j使用CQL“CREATE”命令

- 创建没有属性的节点
- 使用属性创建节点
- 在没有属性的节点之间创建关系
- 使用属性创建节点之间的关系
- 为节点或关系创建单个或多个标签

#### CQL创建一个没有属性的节点

```
CREATE (<node-name>:<label-name>)
```

语法说明

| 语法元素      | 描述                       |
| ------------- | -------------------------- |
| CREATE        | 它是一个Neo4j CQL命令。    |
| \<node-name>  | 它是我们要创建的节点名称。 |
| \<label-name> | 它是一个节点标签名称       |

```
例子：
创建一个简单的“Employee”节点
CREATE (emp:Employee)
这里 emp 是一个节点名
Employee 是 emp 节点的标签名称
```

#### Neo4j CQL创建具有属性的节点

Neo4j CQL“CREATE”命令用于创建带有属性的节点。 它创建一个具有一些属性（键值对）的节点来存储数据。

```
CREATE (
   <node-name>:<label-name>
   { 	
      <Property1-name>:<Property1-Value>
      ........
      <Propertyn-name>:<Propertyn-Value>
   }
)
```

```
例子：
创建
CREATE (dept:Dept { deptno:10,dname:"Accounting",location:"Hyderabad" })

创建具有一些属性（id，name，sal，deptno）的Employee节点
CREATE (emp:Employee{id:123,name:"Lokesh",sal:35000,deptno:10})

```

### 2.2 MATCH  | RETURN

Neo4j CQL MATCH命令用于 

- 从数据库获取有关节点和属性的数据
- 从数据库获取有关节点，关系和属性的数据

 ```
MATCH 
(
   <node-name>:<label-name>
)
 ```

语法说明

| 语法元素      | 描述                         |
| ------------- | ---------------------------- |
| \<node-name>  | 这是我们要创建一个节点名称。 |
| \<label-name> | 这是一个节点的标签名称       |

**注意-**我们不能单独使用MATCH 从数据库检索数据。 如果我们单独使用它，那么我们将InvalidSyntax错误。

```
！错误例子！ 
#单独使用MATCH命令从数据库检索数据会发生什么
#dept是节点名称
#Dept是emp节点的标签名称

MATCH (dept:Dept)
-->must be RETURN or update

```

Neo4j CQL RETURN子句用于 

- 检索节点的某些属性
- 检索节点的所有属性
- 检索节点和关联关系的某些属性
- 检索节点和关联关系的所有属性

```
RETURN 
   <node-name>.<property1-name>,
   ........
   <node-name>.<propertyn-name>
```

语法说明:

| 语法元素             | 描述                                                         |
| -------------------- | ------------------------------------------------------------ |
| \<node-name>         | 它是我们将要创建的节点名称。                                 |
| \<Property1-name>... | 属性是键值对。 \<Property-name>定义要分配给创建节点的属性的名称 |

```
续上错误例子
如果单独使用RETURN命令从数据库检索数据

RETURN dept.deptno
不能单独使用RETURN子句
```

#### MATCH + RETURN

在Neo4j CQL中，我们不能单独使用MATCH或RETURN命令，因此我们应该合并这两个命令以从数据库检索数据。

Neo4j使用CQL MATCH + RETURN命令 

- 检索节点的某些属性
- 检索节点的所有属性
- 检索节点和关联关系的某些属性
- 检索节点和关联关系的所有属性

```
MATCH Command
RETURN Command
```

```
例子
检索Dept节点的一些属性（deptno，dname）数据

MATCH (dept: Dept)
RETURN dept.deptno, dept.dname

dept是节点名称
这里Dept是一个节点标签名
deptno是dept节点的属性名称
dname是dept节点的属性名

MATCH (dept: Dept)
RETURN dept.deptno,dept.dname,dept.location

MATCH (dept: Dept)
RETURN dept
```

### 2.3 关系相关基础

基于方向性，Neo4j关系被分为两种主要类型。

- 单向关系
- 双向关系

在以下场景中，我们可以使用Neo4j CQL CREATE命令来创建两个节点之间的关系。 这些情况适用于Uni和双向关系。

- 在两个现有节点之间创建无属性的关系
- 在两个现有节点之间创建与属性的关系
- 在两个新节点之间创建无属性的关系
- 在两个新节点之间创建与属性的关系
- 在具有WHERE子句的两个退出节点之间创建/不使用属性的关系

**→**

每个关系（→）包含两个节点

- 从节点
- 到节点

对于节点，它们是两种关系

- 外向关系
- 传入关系

### CREATE 创建标签

Label是Neo4j数据库中的节点或关系的名称或标识符。

我们可以将此标签名称称为关系为“关系类型”。

我们可以使用CQL CREATE命令为节点或关系创建单个标签，并为节点创建多个标签。

**单个标签到节点**

语法： //与之前是一样的

```
CREATE (<node-name>:<label-name>)
```

| S.No. | 语法元素                | 描述                      |
| ----- | ----------------------- | ------------------------- |
| 1     | CREATE 创建             | 它是一个Neo4j CQL关键字。 |
| 2     | \<node-name> <节点名称> | 它是一个节点的名称。      |
| 3     | \<label-name><标签名称> | 这是一个节点的标签名称。  |

```
例子：
为“GooglePlusProfile”节点创建单个标签

CREATE (google1:GooglePlusProfile)

```

**多个标签到节点**

```
CREATE (<node-name>:<label-name1>:<label-name2>.....:<label-namen>)
```

```
为“Cinema”节点创建多个标签名称,我们的客户提供的多个标签名称：Cinema,Film,Movie,Picture。

CREATE (m:Movie:Cinema:Film:Picture)
```

**单个标签到关系**

```
CREATE (<node1-name>:<label1-name>)-
	[(<relationship-name>:<relationship-label-name>)]
	->(<node2-name>:<label2-name>)
```

语法说明

| S.No. | 语法元素                                  | 描述                      |
| ----- | ----------------------------------------- | ------------------------- |
| 1     | CREATE 创建                               | 它是一个Neo4J CQL关键字。 |
| 2     | \<node1-name> <节点1名>                   | 它是From节点的名称。      |
| 3     | \<node2-name> <节点2名>                   | 它是To节点的名称。        |
| 4     | \<label1-name> <LABEL1名称>               | 它是From节点的标签名称    |
| 5     | \<label2-name> <LABEL2名称>               | 它是To节点的标签名称。    |
| 6     | \<relationship-name> <关系名称>           | 它是一个关系的名称。      |
| 7     | \<relationship-label-name> <相关标签名称> | 它是一个关系的标签名称。  |

```
CREATE (p1:Profile1)-[r1:LIKES]->(p2:Profile2)
解释：
p1和profile1是节点名称和节点标签名称“From Node”
p2和Profile2是“To Node”的节点名称和节点标签名称
r1是关系名称
LIKES是一个关系标签名称
```

### 2.4 WHERE

CQL MATCH命令中提供了WHERE子句来过滤MATCH查询的结果。

```
//简单形式
WHERE <condition>
//复杂形式
WHERE <condition> <boolean-operator> <condition>
<boolean-operator>
<condition>：
	<property-name> <comparison-operator> <value>
```

语法说明：

| S.No. | 语法元素                            | 描述                                                         |
| ----- | ----------------------------------- | ------------------------------------------------------------ |
| 1     | WHERE                               | 它是一个Neo4j CQL关键字。                                    |
| 2     | \<property-name> <属性名称>         | 它是节点或关系的属性名称。                                   |
| 3     | \<comparison-operator> <比较运算符> | 它是Neo4j CQL比较运算符之一。请参考下一节查看Neo4j CQL中可用的比较运算符。 |
| 4     | \<value> <值>                       | 它是一个字面值，如数字文字，字符串文字等。                   |

Neo4j支持以下运算符在Neo4j CQL WHERE子句中使用以支持多个条件。

| S.No. | 布尔运算符     | 描述                                   |
| ----- | -------------- | -------------------------------------- |
| 1     | AND            | 它是一个支持AND操作的Neo4j CQL关键字。 |
| 2     | OR             | 它是一个Neo4j CQL关键字来支持OR操作。  |
| 3     | NOT            | 它是一个Neo4j CQL关键字支持NOT操作。   |
| 4     | XOR            | 它是一个支持XOR操作的Neo4j CQL关键字。 |
|       | **比较运算符** |                                        |
| 1     | =              |                                        |
| 2     | <>             | 不等于                                 |
| 3     | <(=)           |                                        |
| 4     | \>(=)          |                                        |

```
例子：
MATCH (emp:Employee)
RETURN emp.empid,emp.name,emp.salary,emp.deptno

MATCH (emp:Employee) 
WHERE emp.name = 'Abc'
RETURN emp

MATCH (emp:Employee) 
WHERE emp.name = 'Abc' OR emp.name = 'Xyz'
RETURN emp
```

**使用WHERE子句创建关系**

在Neo4J CQL中，我们可以以不同的方式创建拖曳节点之间的关系。

- 创建两个现有节点之间的关系
- 一次创建两个节点和它们之间的关系
- 使用WHERE子句创建两个现有节点之间的关系

```
MATCH (<node1-label-name>:<node1-name>),(<node2-label-name>:<node2-name>) 
WHERE <condition>
CREATE (<node1-label-name>)-[<relationship-label-name>:<relationship-name>
       {<relationship-properties>}]->(<node2-label-name>) 
```

```
例子：
//检查存在
MATCH (cust:Customer)
RETURN cust.id,cust.name,cust.dob

MATCH (cc:CreditCard)
RETURN cc.id,cc.number,cc.expiredate,cc.cvv

//使用WHERE子句创建两个现有节点之间的关系。
MATCH (cust:Customer),(cc:CreditCard) 
WHERE cust.id = "1001" AND cc.id= "5001" 
CREATE (cust)-[r:DO_SHOPPING_WITH{shopdate:"12/12/2014",price:55000}]->(cc) 
RETURN r
```

### 2.5 DELETE 

Neo4j使用CQL DELETE子句

- 删除节点。
- 删除节点及相关节点和关系。

**删除节点**

通过使用此命令，我们可以从数据库永久删除节点及其关联的属性。

```
DELETE <node-name-list>
DELETE <node-name1,node-name2,...>

```

```
例子：
MATCH (e: Employee) RETURN e

MATCH (e: Employee) DELETE e
```

**DELETE节点和关系子句语法**

```
DELETE <node1-name>,<node2-name>,<relationship-name>
```

| S.No. | 语法元素             | 描述                                                         |
| ----- | -------------------- | ------------------------------------------------------------ |
| 1.    | DELETE               | 它是一个Neo4j CQL关键字。                                    |
| 2.    | \<node1-name>        | 它是用于创建关系\<relationship-name>的一个结束节点名称。     |
| 3.    | \<node2-name>        | 它是用于创建关系\<relationship-name>的另一个节点名称。       |
| 4.    | \<relationship-name> | 它是一个关系名称，它在\<node1-name>和\<node2-name>之间创建。 |

```
例子：
如何从数据库永久删除节点及其关联节点和关系。
MATCH (cc:CreditCard)-[r]-(c:Customer)RETURN r 

MATCH (cc: CreditCard)-[rel]-(c:Customer) 
DELETE cc,c,rel

```

### 2.6 REMOVE | SET

有时基于我们的客户端要求，我们需要向现有节点或关系添加或删除属性。

我们使用Neo4j CQL **REMOVE**子句来删除节点或关系的现有属性。

我们使用Neo4j CQL **SET**子句向现有节点或关系添加新属性。

Neo4j CQL REMOVE命令用于

- 删除节点或关系的标签
- 删除节点或关系的属性

Neo4j CQL DELETE和REMOVE命令之间的主要区别 

- DELETE操作用于删除节点和关联关系。
- REMOVE操作用于删除标签和属性。

Neo4j CQL DELETE和REMOVE命令之间的相似性

- 这两个命令不应单独使用。
- 两个命令都应该与MATCH命令一起使用。

**删除节点/关系的属性**

基本与DELETE一样 只是这个对的是标签

```
REMOVE <property-name-list>
<property-name-list>：
	<node-name>.<property1-name>,
	<node-name>.<property2-name>, 
	.... 
	<node-name>.<propertyn-name> 

```

| S.No. | 语法元素              | 描述                                                 |
| ----- | --------------------- | ---------------------------------------------------- |
| 1。   | REMOVE                | 它是一个Neo4j CQL关键字。                            |
| 2。   | \<property-name-list> | 它是一个属性列表，用于永久性地从节点或关系中删除它。 |

```
例子：
1.
CREATE(book:Book{id:122,title:"Neo4j",pages:340,price:20})

MATCH (book : Book)
RETURN book

MATCH (book{id:122})
REMOVE book.price
RETURN book

2.
MATCH (dc:DebitCard) 
RETURN dc

MATCH (dc:DebitCard) 
REMOVE dc.cvv
RETURN dc
```

删除节点/关系的**标签**

```
REMOVE <label-name-list> 

<node-name>:<label2-name>, 
.... 
<node-name>:<labeln-name> 
```

```
例子：
MATCH (m:Movie) RETURN m

MATCH (m:Movie) 
REMOVE m:Picture
```

**SET**

Neo4j CQL 已提供 SET 子句来执行以下操作。

- 向现有节点或关系添加新属性
- 添加或更新属性值

```
SET  <property-name-list>
```

```
例子：
MATCH (book:Book)
RETURN book

MATCH (book:Book)
SET book.title = 'superstar'
RETURN book

```

### 2.7 Sorting

CQL在MATCH命令中提供了“ORDER BY”子句，对MATCH查询返回的结果进行排序。

```
ORDER BY  <property-name-list>  [DESC]	 
```

```
例子：
MATCH (emp:Employee)
RETURN emp.empid,emp.name,emp.salary,emp.deptno

MATCH (emp:Employee)
RETURN emp.empid,emp.name,emp.salary,emp.deptno
ORDER BY emp.name

MATCH (emp:Employee)
RETURN emp.empid,emp.name,emp.salary,emp.deptno

MATCH (emp:Employee)
RETURN emp.empid,emp.name,emp.salary,emp.deptno
ORDER BY emp.name DESC
```

### 2.8 UNION

UNION子句

它将两组结果中的公共行组合并返回到一组结果中。 它不从两个节点返回重复的行。

结果列类型和来自两组结果的名称必须匹配，这意味着列名称应该相同，列的数据类型应该相同。

```
<MATCH Command1>
   UNION
<MATCH Command2>
```

```
例子：
信用卡式节点数据
MATCH (cc:CreditCard) RETURN cc
借记卡数据的节点
MATCH (dc:DebitCard) RETURN dc

UNION
//错误示例
如果UNION子句的这两个查询确实有相同的名称或相同的数据类型及其列会发生什么。
MATCH (cc:CreditCard) RETURN cc.id,cc.number
UNION
MATCH (dc:DebitCard)  RETURN dc.id,dc.number
//以上错误 两个查询应具有相同的列名，具有相同的属性名：身份证和号码，但他们有不同的节点名称前缀，CQL提供“AS”子句。

MATCH (cc:CreditCard)
RETURN cc.id as id,cc.number as number,cc.name as name,
   cc.valid_from as valid_from,cc.valid_to as valid_to
UNION
MATCH (dc:DebitCard)
RETURN dc.id as id,dc.number as number,dc.name as name,
   dc.valid_from as valid_from,dc.valid_to as valid_to
```

**UNION ALL**(不过滤重复行)

它结合并返回两个结果集的所有行成一个单一的结果集。它还返回由两个节点重复行。

```
<MATCH Command1>
UNION ALL
<MATCH Command2>
```

```
MATCH (cc:CreditCard)
RETURN cc.id as id,cc.number as number,cc.name as name,
   cc.valid_from as valid_from,cc.valid_to as valid_to
UNION ALL
MATCH (dc:DebitCard)
RETURN dc.id as id,dc.number as number,dc.name as name,
   dc.valid_from as valid_from,dc.valid_to as valid_to
//会比上面多出重复数据
```

### 2.9 LIMIT和SKIP

LIMIT”子句来过滤或限制查询返回的行数。 它修剪CQL查询结果集底部的结果。

如果我们要修整CQL查询结果集顶部的结果，那么我们应该使用CQL SKIP子句。

```
LIMIT <number>
```

```
例子：
本示例演示如何使用CQL LIMIT子句减少MATCH + RETURN查询返回的记录数。
MATCH (emp:Employee) 
RETURN emp

MATCH (emp:Employee) 
RETURN emp
LIMIT 2
//只返回了TOP的两个结果
```

```
SKIP <number>
```

```
示例演示如何使用CQL SKIP子句减少MATCH + RETURN查询返回的记录数。
MATCH (emp:Employee) 
RETURN emp

MATCH (emp:Employee) 
RETURN emp
SKIP 2
//返回Bottom的两个结果
```

### 2.10 MERGE

Neo4j使用CQL MERGE命令

- 创建节点，关系和属性
- 为从数据库检索数据

Neo4j CQL MERGE命令在图中搜索给定模式，如果存在，则返回结果；如果它不存在于图中，则它创建新的节点/关系并返回结果。

```
MERGE (<node-name>:<label-name>
{
   <Property1-name>:<Pro<rty1-Value>
   .....
   <Propertyn-name>:<Propertyn-Value>
})
```

Neo4j CQL MERGE命令语法与CQL CREATE命令类似。

我们将使用这两个命令执行以下操作 - 

- 创建具有一个属性的配置文件节点：Id，名称
- 创建具有相同属性的同一个Profile节点：Id，Name
- 检索所有Profile节点详细信息并观察结果

```
通过使用CREATE，MATCH和RETURN命令创建Google+个人资料，执行上述所有操作。
1.创建具有属性：Id，Name的Profile节点
CREATE(gp1:GoogleProfile1{id:201401,Name:"Apple"})

2.创建具有相同属性的同一个Profile节点：Id，Name。
CREATE (gp1:GoogleProfile1 {Id: 201401, Name:"Apple"})

3.检索所有Profile节点详细信息并观察结果。
MATCH  (gp1:GoogleProfile1) 
RETURN gp1.Id,gp1.Name
显示2行重复的值。
CQL CREATE命令检查此节点是否可用，它只是在数据库中创建新节点。
通过观察这些结果，我们可以说CREATE命令总是向数据库添加新的节点。

//==对比merge
MERGE (gp2:GoogleProfile2{ Id: 201402,Name:"Nokia"})

MERGE (gp2:GoogleProfile2{ Id: 201402,Name:"Nokia"})

MATCH  (gp2:GoogleProfile2) 
RETURN gp2.Id,gp2.Name
查询结果只显示一行，因为CQL MERGE命令检查该节点在数据库中是否可用。 如果它不存在，它创建新节点；否则，它不创建新的。

```

CQL MERGE命令将新的节点添加到数据库，只有当它不存在。

### 2.11 NULL

Neo4j CQL将空值视为对节点或关系的属性的缺失值或未定义值。

当我们创建一个具有现有节点标签名称但未指定其属性值的节点时，它将创建一个具有NULL属性值的新节点。

```
WHERE <node.pro> IS NULL
```



### 2.12 IN

Neo4j CQL提供了一个IN运算符，以便为CQL命令提供值的集合。

```
IN[<Collection-of-values>]
```

语法说明：

| S.No. | 语法元素                | 描述                                  |
| ----- | ----------------------- | ------------------------------------- |
| 1。   | IN                      | 它是一个Neo4j CQL关键字。             |
| 2。   | [                       | 它告诉Neo4j CQL，一个值的集合的开始。 |
| 3。   | ]                       | 它告诉Neo4j CQL，值集合的结束。       |
| 4。   | \<Collection-of-values> | 它是由逗号运算符分隔的值的集合。      |

```
如何使用IN运算符检索Employee节点详细信息。
MATCH(e:Employee)
RETURN e.id,e.name,e.sal,e.deptno

MATCH(e:Employee)
WHERE e.id IN [123,124]
RETURN e.id,e.name,e.sal,e.deptno
此查询仅返回在IN运算符中指定的id匹配的两行。

```

例子：

```
例子：
如何使用属性和这两个节点之间的关系创建两个节点。

注-我们将创建两个节点：客户节点 (Customer) 和信用卡节点 (CreditCard)。
客户节点包含：ID，姓名，出生日期属性
CreditCard节点包含：id，number，cvv，expiredate属性
客户与信用卡关系：DO_SHOPPING_WITH
CreditCard到客户关系：ASSOCIATED_WITH

1、创建客户节点
CREATE (e:Customer{id:"1001",name:"Abc",dob:"01/10/1982"})

2、创建CreditCard节点
CREATE (cc:CreditCard
{id:"5001",number:"1234567890",
cvv:"888",expiredate:"20/17"})

3、观察节点
现在我们创建了两个节点：Customer和CreditCard
我们需要使用带有RETURN子句的Neo4j CQL MATCH命令查看这两个节点的详细信息
MATCH (e:Customer)
RETURN e.id,e.name,e.dob

MATCH (cc:CreditCard)
RETURN cc.id,cc.number,cc.cvv,cc.expiredate

```

### 2.12 ID属性 | Caption标题

在Neo4j中，“Id”是节点和关系的默认内部属性。 这意味着，当我们创建一个新的节点或关系时，Neo4j数据库服务器将为内部使用分配一个数字。 它会自动递增。

```
例子：
CREATE (tweet:Tweet{message:"Hello"})
MATCH (tweet:Tweet{message:"Hello"})
RETURN tweet
查看其ID = 0
再执行一次 
CREATE (tweet:Tweet{message:"Hello"})
在查看 1
```

在Neo4j数据中，当我们在Neo4j DATA浏览器中执行MATCH + RETURN命令以查看UI视图中的数据时，通过使用它们的Id属性显示节点和/或关系结果。 它被称为“CAPTION”的id属性。

```
执行MATCH + RETURN命令以查看UI视图中的数据时，它通过id属性显示节点和/或关系结果为“CAPTION”。
MATCH (tweet:Tweet{message:"Hello"})
RETURN tweet

如何更改节点或Neo4j数据浏览器UI视图中的关系的“CAPTION”。
MATCH (tweet:Tweet{message:"Hello"})
RETURN tweet
在UI界面下更改样式选为message
```

### 2.13 方向关系

在Neo4j中，两个节点之间的关系是有方向性的。 它们是单向或双向的。

由于Neo4j遵循属性图数据模型，它应该只支持方向关系。 如果我们尝试创建一个没有任何方向的关系，那么Neo4j DB服务器应该抛出一个错误。

```
所有Neo4j关系是有方向性的。
//错误
CREATE (n1:Node1)-[r1:Relationship]-(n2:Node2)

CREATE (n1:Node1)-[r1:Relationship]->(n2:Node2)
显示Neo4j CQL CREATE命令仅支持方向关系。
```



