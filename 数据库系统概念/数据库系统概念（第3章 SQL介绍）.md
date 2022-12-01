[TOC](第3章 SQL介绍)

`数据库系统概念（第6章 关系代数）`<https://www.cnblogs.com/kirin-dev/p/Database-System-Concepts_Chapter-6.html>

# 3.1 SQL查询语言概览

SQL(Structured Query Language, 结构化查询语言)
SQL语言有几个部分：

- 数据定义语言(Data-Definition Language, DDL)——create/drop/alter/desc
- 数据操纵语言(Data-Manipulation Language, DML)——select/update/insert/delete
- 数据控制语言(Data-Control Language, DCL)——commit/rollback/grant/rewoke
- 完整性(integrity)
- 视图定义(view definition)
- 事务控制(transaction control)
- 嵌入式SQL(embedded SQL)和动态SQL(dynamic SQL)
- 授权(authorization)

# 3.2 SQL数据定义

数据库中的关系集合是用数据定义语言(DDL)定义的。SQL DDL不仅能够定义关系的集合，还能够定义有关每个关系的信息，包括：

- 每个关系的模式
- 每个属性的取值类型
- 完整性约束
- 为每个关系维护的索引集合
- 每个关系的安全性和权限信息
- 每个关系在磁盘上的物理存储结构

## 3.2.1 基本类型

SQL标准支持多种固有类型：

- **char**(n)：具有用户指定长度n的固定长度的字符串。也可以使用全称**character**。
- **varchar**(n)：具有用户指定的最大长度n的可变长度的字符串。等价的全称形式是**character varying**。
- **int**：整数（依赖于机器的整数的有限子集），等价的全称形式是integer。
- **smallint**：小整数（依赖于机器的整数类型的子集）。
- **numeric(p, d)**：具有用户指定精度的定点数。这个数有p位数字（加上一个符号位），并且小数点右边有p位中的d位数字。
- **real, double precision**：浮点数与双精度浮点数，精度依赖于机器。
- **float**(n)：精度至少为n位数字的浮点数。

每种类型都可能包含一个被称作**空**(null)值的特殊值。（null值的存在不总是好的）

## 3.2.2 基本模式定义

我们通过使用**create table**命令来定义SQL关系。
create table命令的通用形式是：

```SQL
create table r
 (A_1 D_1,
 A_2 D_2,
 …,
 A_n D_n,
 <完整性约束_1>,
 …,
 <完整性约束_k>);
```

其中r是关系名，每一个$A_i$是关系r的模式中的一个属性名，$D_i$是属性$A_i$的域；也就是说，$D_i$指定了属性$A_i$的类型以及可选的约束，用于限制所允许的$A_i$取值的集合。
典型的约束有以下三种：

- **primary key($A_{j1}$, $A_{j2}$, …, $A_{jm}$)**：**主码**声明表示属性$A_{j1}$, $A_{j2}$, …, $A_{jm}$构成关系的主码。主码属性必须是非空且唯一的。
- **foreign key ($A_{k1}$, $A_{k2}$, …, $A_{kn}$) references s**：**外码**声明表示关系中任意元组在属性($A_{k1}$, $A_{k2}$, …, $A_{kn}$)上的取值必须对应于关系s中某元组在主码属性上的取值。（包括MySQL在内的一些数据库系统需要显式列出被引用表s中的被引用属性）
- **not null**：一个属性上的**非空**约束表明在该属性上不允许存在空值。

如果要从SQL数据库中去掉一个关系，我们使用**drop table**命令。**drop table**命令从数据库中删除关于被去掉关系的所有信息。**drop table**命令的格式为：

```SQL
drop table r;
```

> 命令**drop table** r;是比**delete from** r;更强的语句。
> 后者保留关系r，但删除r中的所有元组。前者不仅删除r中的所有元组，还删除r的模式。一旦r被去掉，除非用**create table**命令重新创建r，否则没有元组可以插入r中。

如果要为已有关系增加属性，我们使用**alter table**命令。关系中的所有元组在新属性上的取值将被赋为null。**alter table**命令的格式为：

```SQL
alter table r add A D;
```

其中r是现有关系的名称，A是待添加属性的名称，D是待添加属性的类型。
同理，如果要从关系去掉属性，我们可以使用如下命令：

```SQL
alter table r drop A;
```

其中r是现有关系的名称，A是关系的一个属性的名称。（很多数据库系统并不支持去掉属性，尽管它们允许去掉整张表）

# 3.3 SQL查询的基本结构

SQL查询的基本结构由三个子句构成：**select**、**from**和**where**。查询以在**from**子句中列出的关系作为其输入，在这些关系上进行**where**和**select**子句中指定的运算，然后产生一个关系作为结果。

## 3.3.1 单关系查询

```SQL
select A
from r;
```

在某些情况下如果想强行去除重复，可以在**select**后插入关键字**distinct**：

```SQL
select distinct A
from r;
```

SQL同样允许使用关键字**all**来显式指名不去除重复：

```SQL
select all A
from r;
```

**select**子句还可带含有+、-、\*、/运算符的算数表达式，运算对象可以是常数或元组的属性（不导致r关系发生任何改变）：

```SQL
select A * 1.1
from r;
```

**where**子句允许我们只选出那些在**from**子句的结果关系中满足特定谓词的元组：

```SQL
select A
from r
where condition1 and/or/not condition2…;
```

SQL允许在**where**子句中使用逻辑连词**and**、**or**和**not**。逻辑连词的运算对象可以是包含比较运算符<、<=、>、>=、=和<>的表达式。

## 3.3.2 多关系查询

类似的，我们给出一个典型的SQL查询：

```SQL
select A_1, A_2, …, A_n
from r_1, r_2, …, r_m
where P;
```

每个$A_i$代表一个属性，每个$r_i$代表一个关系。P是一个谓词。如果省略**where**子句，则谓词P为**真**。
**from**子句定义了一个在该子句中所列出关系上的笛卡尔积，具有来自**from**子句中所有关系的所有属性。（当关系$r_i$和$r_j$中出现相同的属性名，需要在属性名前加上该属性所来自的那个关系的名称作为前缀）
**where**子句使用谓词来限制笛卡尔积所创建的组合。（实际编码过程中，我们总会使用**where**子句，否则会导致输出的元组数量巨大）
> 通常说来，一个SQL查询的含义可以理解如下：
>
> 1. 为**from**子句中列出的关系产生笛卡尔积。
> 2. 在步骤1的结果上应用**where**子句中指定的谓词。
> 3. 对于步骤2的结果中的每个元组，输出**select**子句中指定的属性（或表达式的结果）。
>
## 查询语句与关系代数

上述的SQL查询语句等价于多重集关系代数表达式：
$\prod_{A_1, A_2, …, A_n}(\sigma_P(r_1 × r_2 × … × r_m))$

# 3.4 附加的基本运算

## 3.4.1 更名运算

SQL提供重命名结果关系中的属性的方式——**as**子句（也可以使用空格）

```SQL
old-name as new-name
```

- 把一个长的关系名替换成短的，使得查询中的其他地方使用起来更为方便
- 适用于需要比较同一个关系中的元组的情况（被用来重命名关系的标识在SQL标准中被称作**相关名称**(correlation name)或**表别名**(table alias)或**相关变量**(correlation variavle)或**元组变量**(tuple variable)）

## 3.4.2 字符串运算

在SQL标准中，字符串上的相等运算是**大小写敏感**的。（然而一些数据库系统，如MySQL和SQL Server，在匹配字符串时并不区分大小写）
SQL还允许在字符串上**作用多种函数**，例如连接字符串(“||”)、提取子串、计算字符长度、大小写转换(“upper”、“lower”)、去掉字符串后面的空格(“trim”)等。
字符串上可以使用**like**运算符来实现模式匹配。我们使用两个特殊的字符来描述模式（模式是大小写敏感的）：

- 百分号(%)：%字符匹配任意字符串。
- 下划线(\_)：\_字符匹配任意一个字符
为使模式能够包含特殊的模式字符(%、\_)，SQL允许定义转义字符。转义字符直接用在特殊的模式字符的前面，表示该特殊的模式字符被当成普通字符。我们在**like**比较运算中使用**escape**关键字来定义转义字符。（eg：like 'ab\%cd%' escape）
SQL允许使用**not like**比较运算符来搜索不匹配项。

## 3.4.3 select子句中的属性说明

星号“\*”可以用在**select**子句中表示“所有的属性”。
形如select \*的**select**子句表示**from**子句的结果关系的所有属性都被选中。

## 3.4.4 排列元组的显示次序

**order by**子句可以让查询结果中的元组按排列顺序显示。
要说明排序顺序，我们可以用**desc**表示降序，或用**asc**表示升序。（在缺省的情况下，默认升序）

## 3.4.5 where子句谓词

SQL提供**between**比较运算符来说明一个值在某个区间内。
类似地，还可以使用**not between**比较运算符。
SQL还允许用符号($v_1, v_2, …, v_n$)来表示一个n维元组，该符号被称为行构造器(row constructor)。在元组上可以运用比较运算符。（eg：当$a<b$且$c<d$时，$(a, c) < (b, d)$）

# 3.5 集合运算

SQL作用在关系上的**union**、**interest**和**except**运算对应于数学集合论中的$\cup$、$\cap$和$-$运算。

## 3.5.1 并运算

```SQL
select A_1, A_2, …, A_n
from r_1, r_2, …, r_m
where P;
union
select B_1, B_2, …, B_n
from s_1, s_2, …, s_m
where Q;
```

> **union**运算自动去除重复。如果想保留所有重复项，用**union all**代替**union**。

## 3.5.2 交运算

```SQL
select A_1, A_2, …, A_n
from r_1, r_2, …, r_m
where P;
intersect
select B_1, B_2, …, B_n
from s_1, s_2, …, s_m
where Q;
```

> **intersect**运算自动去除重复。如果想保留所有重复项，用**intersect all**代替**intersect**。

## 3.5.3 差运算

```SQL
select A_1, A_2, …, A_n
from r_1, r_2, …, r_m
where P;
except
select B_1, B_2, …, B_n
from s_1, s_2, …, s_m
where Q;
```

> **except**运算自动去除重复。如果想保留所有重复项，用**except all**代替**except**。

# 3.6 空值

空值(null value)给包括算术运算、比较运算和集合运算在内的关系运算带来了特殊的问题。
SQL将涉及空值的任何比较运算的结果视为**unknown**，这创建了除**true**和**false**之外的第三种逻辑值。
由于**where**子句中的谓词可以对比较结果使用诸如**and**、**or**和**not**的布尔运算，因此这些布尔运算的定义也被扩展为可以处理**unknown**值。

- **and**：true and unknown的结果是unknown；false and unknown的结果是false；unknown and unknown的结果是unknown。
- **or**：true or unknown的结果是true；false or unknown的结果是unknown；unknown or unknown的结果是unknown。
- **not**：not unknown的结果是unknown。

> 如果**where**子句谓词对一个元组计算出**false**或**unknown**，那么该元组不能被加入结果中。

SQL允许使用**is null/is not null**来测试空值。
SQL允许使用**is unknown/is not unknown**来测试一个比较运算的结果是否为**unknown**。

# 3.7 聚集函数

**聚集函数**(aggregate function)是以值集（集合或多重集合）为输入并返回单个值的函数。
SQL提供了如下五个标准的固有聚集函数：

- 平均值：avg。
- 最小值：min。
- 最大值：max。
- 总和：sum。
- 计数：count。

> **sum**和**avg**的输入必须是数字集，其他运算符可以作用在非数字数据类型的集合上。

## 3.7.1 基本聚集

如果想去除/保留重复项，可在聚集表达式中使用关键字**distinct**/**all**。
我们经常使用聚集函数**count**来计算一个关系中元组的数量：

```SQL
select count(*)
from r;
```

> SQL不允许在用**count**(\*)时使用distinct。

## 3.7.2 分组聚集

SQL允许使用**group by**子句来将聚集函数作用在一组元组集上。
**group by**子句中给出的一个或多个属性是用来构造分组的。在**分组**(group by)子句中的所有属性上取值相同的元组将被分在一个组内。
> 当SQL查询使用分组时，一个很重要的事情是确保出现在**select**语句中但没有被聚集的属性只能是出现在**group by**子句中的那些属性。换句话说，任何没有出现在**group by**子句中的属性如果出现在**select**子句中，它只能作为聚集函数的参数，否则这样的查询就是错误的。

## 3.7.3 having子句

SQL允许使用**having**子句来对分组限定条件。
> SQL在形成分组之后才应用**having**子句中的谓词，因此在**having**子句中可以使用聚集函数。

与**select**子句的情况类似，任何出现在**having**子句中，但没有被聚集的属性必须出现在**group by**子句中，否则查询就是错误的。

包含聚集、**group by**或**having**子句的查询的含义可通过下述运算序列来定义：

1. 与不带聚集的查询情况类似，首先根据from子句来计算出一个关系。
2. 如果出现了where子句，where子句中的谓词将应用到from子句的结果关系上。
3. 如果出现了group by子句，满足where谓词的元组通过group by子句被放入分组中。如果没有group by子句，满足where谓词的整个元组集被当成一个元组。
4. 如果出现了having子句，它将应用到每个分组上；不满足having子句谓词的分组将被去掉
5. select子句利用剩下的分组产生查询结果中的元组，即在每个分组上应用聚集函数来得到单个结果元组。

## 3.7.4 对空值和布尔值的聚集

聚集函数处理空值的原则：除了**count**(\*)之外所有的聚集函数都忽略其输入集合中的空值。因此聚集函数的输入值集合可能为空集，此时我们规定空集的**count**运算值为0，并且当作用在空集上时，其他所有聚集运算返回一个空值。
**布尔**(boolean)数据类型包含**true**、**false**和**unknown**三种值。聚集函数**some**和**every**可应用于布尔值的集合，并分别计算这些值的析取(or)与合取(and)。

# 3.8 嵌套子查询

SQL提供嵌套子查询机制。子查询是嵌套在另一个查询中的**select-from-where表达式**。
> 来自外层查询的**相关名称**（更名）可以用在子查询中。
> 使用了来自外层查询的相关名称的子查询被称作**相关子查询**(correlated subquery)。

## 3.8.1 集合成员资格

SQL允许测试元组在关系中的成员资格。
连接词**in**测试集合成员资格，连接词**not in**测试集合成员资格的缺失。（这里的集合是由select子句产生的一组值构成的）

```SQL
select A_1, A_2, …, A_n
from r_1, r_2, …, r_m
where P and
(B_1, B_2, …, B_n) (not) in (select B_1, B_2, …, B_n
     from s_1, s_2, …, s_m
     where Q);
```

> in和not in运算符也能用于枚举集合。

## 3.8.2 集合比较

嵌套子查询能够对集合进行比较。

```SQL
select A_1, A_2, …, A_n
from r_1, r_2, …, r_m
where P and
A > some/all (select A
   from s_1, s_2, …, s_m
   where Q);
```

> 上述查询中的比较可以替换为<some/all、<=some/all、>=some/all、>some/all、<>some/all
> =some等价于in，<>some不等价于not in。
> <>all等价于not in，=all不等价于in。

## 3.8.3 空关系测试

SQL可测试一个子查询的结果中是否存在元组。
**exists**结构在作为参数的子查询非空时返回**true**值。
同理，可以使用**not exists**结构来测试子查询结果集中是否不存在元组。

```SQL
select A_1, A_2, …, A_n
from r_1, r_2, …, r_m
where P and
(B_1, B_2, …, B_n) (not) exists (select B_1, B_2, …, B_n
     from s_1, s_2, …, s_m
     where Q);
```

> 我们可以使用not exists结构来模拟集合包含（即超集）运算：可将“关系A包含关系B”写成“not exists(B except A)”。

## 3.8.4 重复元组存在性测试

SQL提供布尔函数**unique**，用于测试在一个子查询的结果中是否存在重复元组，如果不存在则返回**true**值。
同理，也可以使用**not unique**结构测试在一个子查询结构中是否存在重复元组。

```SQL
select A_1, A_2, …, A_n
from r_1, r_2, …, r_m
where P and
unique (select B_1, B_2, …, B_n
     from s_1, s_2, …, s_m
     where Q);
```

> 当且仅当在关系中存在两个元组$t_1$和$t_2$使得$t_1 = t_2$，该关系上的**unique**测试被定义为假。
> 如果$t_1$或$t_2$的某些域为空，因为对$t_1 = t_2$的测试为假，所以即使存在一个元组的多个副本，只要该元组至少有一个属性为空，那么**unique**测试结果就有可能为真。

## 3.8.5 from子句中的子查询

SQL允许在**from**子句中使用子查询表达式——任何**select-from-where**表达式返回的结果都是关系，因而可以被插入到另一个**select-from-where**中关系可以出现的任何位置。

```SQL
select A_1, A_2, …, A_n
from (select B_1, B_2, …, B_n
  from r_1, r_2, …, r_m
  where Q)
where P;
```

> 大多数的SQL实现都支持在**from**子句中嵌套子查询。
> 某些SQL实现（MySQL、PostgreSQL）要求**from**子句中的每个子查询的结果关系必须被命名，即使此名称从未被引用。
> Oracle允许对子查询的结果关系命名，但不支持对此关系的属性更名。

> SQL如今允许**from**子句中的子查询用关键字**lateral**作为前缀，以便访问同一个**from**子句中在它前面的表或子查询的属性。

## 3.8.6 with子句

**with**子句提供了一种定义临时关系的方式，这个定义只对包含**with**子句的查询有效。

```SQL
with S(C_1, C_2, …, C_n) as
 (select B_1, B_2, …, B_n
  from s_1, s_2, …, s_m
  where Q)
select A_1, A_2, …, A_n
from r_1, r_2, …, r_m, S
where P;
```

> 此临时关系只能在同一查询的后面部分中使用。

## 3.8.7 标量子查询

SQL允许子查询出现在返回单个值的表达式能够出现的任何地方，只要该子查询只返回一个包含单个属性的元组，这样的子查询称为**标量子查询**(scalar subquery)。
标量子查询可以出现在**select**、**where**和**having**子句中。
> 例如，不带**group by**的**count**(\*)聚集函数只返回单个值，可以被用到**select**子句中。

## 3.8.8 不带from子句的标量

某些查询需要计算，但不需要引用任何关系。
类似的，某些查询可能有包含**from**子句的子查询，但高层查询不需要**from**子句。
![image](https://img2022.cnblogs.com/blog/2975286/202210/2975286-20221006214204315-1117655698.jpg)

# 3.9 数据库的修改

## 3.9.1 删除

> 只能删除整个元组，不能只删除某些属性上的值

```SQL
delete from r
where P;
```

其中，$P$代表一个谓词，而$r$代表一个关系。
> 一条**delete**命令只能作用于一个关系，如果想从多个关系中删除元组，必须为每个关系使用一条命令。

> 工程中**where**子句一般不建议省略，这样会导致删除关系中的所有元组，此过程不可逆。（工程中甚至不建议使用**delete**子句，改为采用软删除的方式，此时被软删除的元组的state(boolean类型)字段被设置为0，我们还能访问已被删除的元组）

> 在执行任何删除之前先进行所有元组的测试是至关重要的，如果一些元组在另一些元组未被测试前先被删除，例如聚集函数计算出的值将会改变，导致严重后果——**delete**的结果依赖于元组被处理的顺序。

## 3.9.2 插入

```SQL
insert into r
 values(V_1, V_2, …, V_n);
```

> 待插入元组的属性值必须在相应属性的域中。待插入元组的属性数量必须正确。待插入元组的属性排列顺序必须与关系模式中一致。

> 如果存在嵌套子查询，那么在执行任何插入之前必须先执行完嵌套子查询中的**select**语句，否则会插入无数相同的元组。

## 3.9.3 更新

```SQL
update r
set result / A = case
where P;
```

> 同样的，必须先检查关系中的所有元组，看它们是否应该被更新，然后才执行更新。

> 使用多条**update**语句时，语句的顺序十分重要。

SQL提供**case**结构，我们可以使用它在单条**update**语句中。

```SQL
case
 when pred_1 then result_1
 when pred_2 then result_2
 …
 when pred_n then result_n
 else result_0
```

> 标量子查询在SQL更新语句中非常有用，它们可以用在**set**子句中。

# 3.10 总结
