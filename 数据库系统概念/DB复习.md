# Exam

40'~50'选择题：

- 基本概念 Chapter概述 什么是关键字、主关键字、外关键字√
- 给SQL语句 选查询结果/给查询结果 选SQL语句√
- 事务隔离集：描述一个不可重复读的现象，要求可复读，如何设置隔离集√
- 给函数依赖集 判断范式√
- 无损连接性判断 分解成两个关系 R1的属性全集交R2的属性全集能不能推导出R1或R2√
- 死锁判断√
- 依赖图是否冲突可串行化√
- Chapter事务恢复：锁的兼容性(PPT图) S、X、IS、IX锁√

60'~50'简答题：

- 25' SQL 最简单的SQL+嵌套的SQL+分组查询+TRIGGER√
- 5'  关系代数 查询(投影+选择)√/约束(关键字、外关键字)√
- 5'  范式理论证明 分解/合并规则√
- 8'  三范式分解 最小覆盖集+无损连接性判断√
- 5'  ER图 给需求画图√
- 5'  权限图 给GRANT、REVOKE语句画图√
- 7'  数据库恢复过程(Analysis-Redo-undo)&WAL(Write-Ahead Logging)协议√

Point:SQL、范式、事务管理

## SQL

### DDL

```SQL
CREATE TABLE <table name> (
    <attribute name> DOMAIN CONSTRAINT,
    ...,
    PRIMARY KEY (<attribute name>, ...),
    FOREIGN KEY (<attribute name>, ...) REFERENCES <table name> (<attribute name>, ...)
);
ALTER TABLE <table name> ADD <attribute name> DOMAIN CONSTRAINT;
ALTER TABLE <table name> DROP <attribute name>;
DROP TABLE <table name>;
```

### DML

```SQL
SELECT [ALL|DISTINCT] A_1, A_2, …, A_n
FROM R_1, R_2, …, R_m
[WHERE P]
[GROUP BY A [HAVING Q]]
[ORDER BY A [ASC|DESC]];

INSERT INTO R(A_1, A_2, …, A_n) VALUES(V_1, V_2, …, V_n);

DELETE FROM R WHERE P;

UPDATE R SET RESULT/A = case WHERE P;
```

> AS 改名
>
> LIKE/NOT LIKE 字符串匹配_%
>
> IS NULL/IS NOT NULL 判断空值
>
> UNKNOWN 第三个布尔值
>
> EXISTS 判断子查询结果是否为空
>
> IN 判断值是否在子查询结果中
>
> \>ALL 判断值是否大于子查询结果中任意一个值
>
> \<ANY 判断值是否小于子查询结果中至少一个值
>
> A_1 JOIN A_2 ON P ($\theta$)连接
>
> NATURAL JOIN 自然连接
>
> NATURAL FULL/LEFT/RIGHT OUTER JOIN 完全/左/右外连接
>
> INTERSECT/UNION/EXCEPT 交/并/差
>
> ORDER BY 对结果进行排序
>
> GROUP BY 分组
>
> SUM/AVG/MIN/MAX/COUNT 聚集函数(NULL是否计算？)
>
> HAVING 限制分组

## 事务管理(第一本书)

数据库事务的四大特性ACID：

- 原子性：事务包含的所有数据库操作要么全部成功，要不全部失败回滚。
- 一致性：一个事务执行之前和执行之后都必须处于一致性状态。拿转账来说，假设用户A和用户B两者的钱加起来一共是5000，那么不管A和B之间如何转账，转几次账，事务结束后两个用户的钱相加起来应该还得是5000，这就是事务的一致性。
- 隔离性：一个事务未提交的业务结果是否对于其它事务可见。级别一般有：read_uncommit，read_commit，read_repeatable，串行化访问。
- 持久性：一个事务一旦被提交了，那么对数据库中数据的改变就是永久性的，即便是在数据库系统遇到故障的情况下也不会丢失提交事务的操作。

### 脏读

脏数据是还没有COMMIT的事务所写的数据。

一个事务正在对一条记录做修改，在这个事务完成并提交之前，这条数据是处于待定状态的（可能提交也可能回滚），这时，第二个事务来读取这条没有提交的数据，并据此做进一步的处理，就会产生未提交的数据依赖关系。这种现象被称为脏读。

风险：写数据的事务可能夭折；脏数据将被移除。

偶尔脏读可以避免：DBMS耗时防止脏读；等到不可能出现脏读导致并发性损失。

### 不可重复读

一个事务先后读取同一条记录，而事务在两次读取之间该数据被其它事务所修改，则两次读取的数据不同，称之为不可重复读。

### 幻读

一个事务按相同的查询条件重新读取以前检索过的数据，却发现其他事务插入了满足其查询条件的新数据，称之为幻读。

> 不可重复读和脏读的区别是：脏读是某一事务读取了另一个事务未提交的脏数据，而不可重复读则是读取了前一事务提交的数据。
>
> 幻读和不可重复读都是读取了另一条已经提交的事务（这点就脏读不同），所不同的是不可重复读查询的都是同一个数据项，而幻读针对的是一批数据整体（比如数据的个数）。

### 隔离层次

可串行化(默认)：SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;

读未提交：SET TRANSACTION READ WRITE
ISOLATION LEVEL READ UNCOMMITTED;

读提交：SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

可重复读：SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

![img](https://img2023.cnblogs.com/blog/2975286/202211/2975286-20221128145432300-1851534353.png)

## 关系代数

- 并、交、差
- 除去某些行(选择)/某些列(投影)
- 组合两个关系元组(笛卡尔积/连接)
- 重命名

### 并$\cup$、交$\cap$、差$-$

与三种操作在集合上的定义类似。

### 投影$\Pi$

$\Pi_{A_1, A_2, \dots}(R)$表示关系$R$中属性$A_1, A_2, \dots$所代表的列的集合。

### 选择$\sigma$

$\sigma_{C}(R)$表示关系$R$中满足条件$C$的元组的集合。

### 笛卡尔积$\times$

$R \times S$表示由关系$R$和关系$S$中各取一个元组组成的有序对的集合。

> 如果有同样的属性$B$，则用$R.B$和$S.B$对其进行区分。

### 自然连接$\bowtie$

$R \bowtie S$表示由关系$R$和关系$S$同样的属性且值相同的元组组成的有序对的集合。

> 外连接：两个关系中所有元组都必须出现，空缺值用NULL填补。
>
> 左外连接：左边关系中所有元组都必须出现，空缺值用NULL填补。
>
> 右外连接：右边关系中所有元组都必须出现，空缺值用NULL填补。

### $\theta$连接$\bowtie_C$

$R \bowtie_C S$表示在$R \times S$中满足条件$C$的元组的集合。

### 除法$\div$

$R \div S$表示关系$R$的元组中除去与关系$S$属性相同且值相同的值的元组的集合。

### 重命名$\rho$

$\rho_{S(A_1, A_2, \dots)}(R)$表示将关系$R$重命名为$S$，且属性名重命名为$A_1, A_2, \dots$。

### 使用关系代数表示约束

- $R = \varnothing$、$R \subset S$
- 域约束
- 键约束($\sigma_{R1.pk = R2.pk AND R1.A \neq R2.A}(R1 \times R2) = \varnothing$)
- 类似的如何写出外关键字约束？

## 函数依赖与范式

### 函数依赖

$A1...An -> B1...Bn$：如果R的两个元组在属性$A1, ..., An$(键/超键)上一致，则属性$B1, ..., Bn$也一致。

**分解/合并规则**：$A1...An -> B1...Bn$ <=> $A1...An -> Bi$

> 10.07 37min期末考试考合并规则的证明(用Armstrong公理的增广律)

属性的闭包：属性闭包中的存在的任意属性组合都可以由该属性推出。eg:$\{A, B\}^+ = \{A, B, C, F\}$可以得出$AB->CF$等

最小化基本集/函数依赖集：右边为单一属性/删除任一FD则不再是基本集/删除左边任何属性则不再是基本集(求法：除本求包)

函数依赖集的投影(求在投影中成立的FD集)

**分解**(目的：消除异常)：将关系$R(A1, ..., An)$分解为关系$S(B1, ..., Bn)$和$T(C1, ..., Cn)$。(其中$\{A1, ..., An\} = \{B1, ..., Bn\} \cup \{C1, ..., Cn\}$，$S$和$T$为R的投影)

### 范式

BCNF(BC范式)：非平凡FD左边必须包含键。

> *分解为BCNF*：1. 列出所有右边为单属性的FD2. 判断FD是否违反BCNF(左边闭包是否包含所有属性)3. 对于BCNF违例，将原关系分解为包含左边闭包中属性和不包含左边闭包中属性加上左边属性的两个关系，分别计算FD集4. 递归分解

> **无损连接性判断**：根据分解画出图例，根据FD化简，查看是否有可以确定的一行。
>
> ![img](https://img2023.cnblogs.com/blog/2975286/202211/2975286-20221129175959903-582912949.png)

1NF：原子、有键

2NF：无非主属性的部分函数依赖(确保表中的每列都和主键相关)

3NF：非主属性无传递依赖于key(确保每列都和主键列直接相关，而不是间接相关)

> 判断方法(3步)：部分依赖、传递依赖、左边都是候选键<https://blog.csdn.net/weixin_43865875/article/details/115659734>

> **三范式分解**(要求：3NF/无损连接/依赖保持)：1. 找出FD集的一个最小基本集2. 对于最小基本集中每个FD:X->A，将XA作为分解出的某个关系的模式3. 如果分解出的关系的模式均不包含原关系的超键，则增加一个关系，其模式为原关系的任何一个键

## ER图

- 矩形表示实体集
- 菱形表示关系
- 椭圆表示属性(下划线表示主键)

> 一对一/一对多/多对一/多对多：如何设置箭头？(“一”的地方有箭头)
>
> 弱实体集？弱关系？

## 约束与触发器(不考INSTEAD OF触发器)

### 约束

约束的种类：

- 键约束
- 外键约束
- 基于属性的CHECK约束
- 基于元组的CHECK约束
- 断言

维护引用完整性：

- DEFAULT
- CASCADE
- SET NULL

> ON UPDATE/DELETE CASCADE/SET NULL

> 延迟约束检查：DEFERRABLE INITIALLY DEFERRED/IMMEDIATE

修改约束：

- SET CONSTRAINT XXX DEFERRED/IMMEDIATE;
- ALTER TABLE XXX DROP XXX;
- ALTER TABLE XXX ADD XXX CHECK();
- CREATE ASSERTION XXX CHECK();
- DROP ASSERTION XXX;

### 触发器

事件-条件-动作(ECA)规则

```SQL
CREATE TRIGGER XXX
AFTER/BEFORE UPDATE/INSERT/DELETE [OF A] ON R
REFERENCING
    OLD/NEW ROW/TABLE AS XXX
FOR EACH ROW/STATEMENT
WHEN ()
...;
/
BEGIN
    ...;
END;
```

## 视图与索引(应该只考选择或者不考)

### 视图

视图的种类：

- 虚拟视图
- 物化视图

```SQL
CREATE [MATERIALIZED] VIEW XXX AS
...;
```

### 索引

关系中属性A上的索引是一种数据结构，能提高在属性A上查找具有某个特定值的元组的效率。(查询更多的属性放在前面)

```SQL
CREATE INDEX XXX ON R(A);
```

> 最佳索引？

## GRANT、REVOKE语句、权限图

SQL中定义了九种类型的权限：

- SELECT
- INSERT
- DELETE
- UPDATE
- REFERENCE
- USAGE
- TRIGGER
- EXECUTE
- UNDER

### 授权(GRANT)

```SQL
GRANT 权限列表 ON 数据库元素 TO 用户列表
    [WITH GRANT OPTION];
```

> 授权选项决定了被授权方是否有权限授权他人。

### 收权(REVOKE)

```SQL
REVOKE 权限列表 ON 数据库元素 FROM 用户列表
    CASCADE/RESTRICT;
```

> CASCADE:收回指定的权限时，也要收回由于要收回的权限而被授予的权限。
>
> RESTRICT:如果由于要收回的权限被传递给其他人而造成了任何权限收回，则不被执行。

习题10.1.2~4

## 事务管理(第二本书)

### 依赖图(优先图)是否冲突可串行化

优先图有环，不冲突可串行化。

优先图无环，冲突可串行化。

![img](https://img2023.cnblogs.com/blog/2975286/202211/2975286-20221130175826008-1618053358.png)

![img](https://img2023.cnblogs.com/blog/2975286/202211/2975286-20221130175840370-1320839321.png)

### 死锁判断

![img](https://img2023.cnblogs.com/blog/2975286/202211/2975286-20221130172442430-775258765.png)

### 锁的兼容性

![img](https://img2023.cnblogs.com/blog/2975286/202211/2975286-20221130174028602-1333619546.png)

## 事务恢复

### 数据库恢复过程(Analysis-Redo-undo)

![img](https://img2023.cnblogs.com/blog/2975286/202211/2975286-20221130180228457-1674621549.png)

翻译：

- Analysis：向前扫描日志(从最近的检查点)，以识别所有活动的Xact以及所有崩溃时缓冲池中的脏页。
- Redo：根据需要重做缓冲池中脏页的所有更新，确保所有记录的更新都被正确地执行并写入磁盘。
- Undo：在崩溃时处于活动状态的所有Xact的写入都被撤销(通过更新的日志记录恢复更新之前的值)，在日志中反向工作。(必须小心处理恢复过程中发生崩溃的情况！)

### WAL(Write-Ahead Logging)协议

![img](https://img2023.cnblogs.com/blog/2975286/202211/2975286-20221130180244683-1278629088.png)

- 在事务提交完成之前，所有的这些记录必须被写入硬盘
- 事务所引起的所有改动都要记录在日志中

来自网络：

1. 事务所引起的所有改动都要记录在日志中，在事务提交完成之前，所有的这些记录必须被写入硬盘。

2. 一个数据库的缓冲页直到被记入日志后才能发生修改。直到缓冲页对应的日志被写入磁盘之后，该缓冲页才会存入磁盘。

3. 当缓冲页被修改和日志被更新的时候，在也上必须加上互斥锁，以保证改动被记录到日志中的顺序与它发生的顺序是一致的。
