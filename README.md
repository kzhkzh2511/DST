# 数据库

## 第一章--数据库概述

### 概念

数据（Data），描述事物的符号标记

数据库（DB），存放数据的仓库，具有**永久存储、有组织和可共享**三个基本特点

数据库管理系统（DBMS），位于用户应用程序与操作系统之间，专门用于实现对数据进行管理和维护的系统软件

数据库系统（DBS）,指在计算机中引入数据库后的系统，一般由**数据库、数据库管理系统、应用程序、数据库管理员**组成，为不引起混淆书中一般把DBS称为数据库

### 数据库的诞生

就是最早是用文件管理系统进行管理数据，然后很不方便，巴拉巴拉巴拉。。。

而有了数据库后，用户只需要知道存放所需数据的数据库名，就可以对数据库对应的数据文件中的数据进行操作。

具有如下好处：

- 相互关联的数据集合
- 较少的数据冗余
- **程序与数据相互独立**
- 保证数据安全可靠
- 最大限度地保证数据的正确性
- 数据可以共享并能保证数据的一致性

### 数据独立性

**数据独立性**包括数据的**物理独立性**和数据的**逻辑独立性**

**物理独立性**是指用户的应用程序与存储在磁盘上的数据库中数据是相互独立的。

**逻辑独立性**是指用户的应用程序与数据库的逻辑结构是相互独立的，即，当数据的逻辑结构改变时，用户程序也可以不变。

其实有个问题就是**三级模式**[^1]，外模式、模式、内模式

### 数据库系统

- 硬件 
- 软件
  - 数据库管理系统
  - 支持数据库管理系统运行的操作系统，比如Windows、Linux
  - 以数据库管理系统为核心的使用工具
- 人员
  - 数据库管理员，负责维护整个系统的政策运行，负责保障数据库的安全与可靠
  - 系统分析员。负责应用系统的需求分析和规范说明，参与数据库应用系统的概要设计
  - 数据库设计人员 ，确定数据库数据，设计数据库的结构，一般由数据库管理员兼任

## 第二章--数据模型与数据库结构

### 数据库特征

- 静态特征
  - 包括数据的基本结构、数据间的关系以及对数据取值范围的约束
- 动态特征
  - 指对数据可以进行的操作以及操作规则。（增删查改）

数据模型三要素：描述数据时要包括数据的基本结构、数据的约束条件（**前两个是静态特征**）和定义在数据上的操作（**动态特征**）

### 数据模型

数据模型是一种模型，他是对现实世界数据特征的抽象

``` mermaid
graph LR;
    现实世界-->|抽象|信息世界;
    信息世界-->|转换|机器世界;
    机器世界-->|描述|现实世界;
```

### 概念层（E-R图）

- 实体
  - 具有公共性质，并可相互区分的显示世界对象的集合（比如说职工、学生、课程）
  - 实体中每个具体的记录值（一行数据），称为实体的一个实例，比如：学生实体中的一个具体学生就是学生实体的一个实例
  - **用矩形框表示**
- 属性
  - 每个实体都具有一定的特征或性质，**属性是描述实体或联系的性质或特征的数据项，属于一个实体的所有实例都有相同的性质**，例如学生的学号、姓名、出生日期
  - 在实体属性中，能够唯一标识实体的一个属性或最小的一组属性称为**实体的标识属性**
  - **用圆角矩形表示**
- 联系
  - **用菱形框表示**，用连线将联系框与他所关联的实体连接起来
  - 联系也可以有自己的属性

两个实体的联系之间往往有以下三类：

- 一对一：

``` mermaid
graph TD;
	A[经理]---|1|B{管理}
	B---|1|C[部门]
```



- 一对多：

``` mermaid
graph TD;
	A[部门]---|1|B{工作}
	B---|n|C[职员]
```



- 多对多：

``` mermaid
graph TD;
	A2(学号)---B[学生]
	A1(姓名)---B[学生]
	A3(性别)---B[学生]
	B---|m|C{选课}
	C---D(成绩)
	C---|n|E[课程]
	E---F1(课程号)
	E---F2(课程名)
	E---F3(学分)
```

### 组织层数据模型

#### 层次数据模型

用树状结构表示实体与实体之间的联系，层次模型可以直接方便地表示一对多的关系，但有如下限制：

- 有且仅有一个节点无父节点，这个节点即为树的根
- 其他节点有且仅有一个父节点

模型示意图：

``` mermaid
graph TD;
	R1-->R11
	R1-->R12
	R11-->R21
	R11-->R22
	R11-->R23
```



#### 网状数据模型

用网形结构表示实体与实体之间的联系的数据模型

#### 关系数据模型（最常用）

用关系（表格数据）表示实体与实体之间的联系的数据模型，与前面模型相比最根本的区别是：**关系数据模型采用非导航式的数据访问方式，数据结构的变化不会影响对数据的访问**

模型示意图：

| 学号 | 课程号 | 成绩 |
| :--: | :----: | :--: |
| 001  |  C003  |  33  |
| 002  |  C002  |  23  |
| 003  |  C005  |  13  |

### 数据库架构---三级模式架构

``` mermaid
graph TD;
外模式-->|映像|模式-->|映像|内模式
```

- **模式**
  - 是数据库中全体数据的逻辑结构和特征的描述，它仅仅涉及到“型”的描述，不涉及具体的值
  - 模式的一个具体值称为模式的一个事例
  - 数据模式描述一类事物的结构、属性、类型和约束，实质上是用数据模型对一类事物进行模拟，而事例是反映某类事物在某一时刻的当前状态
  - 描绘着一个部门或公司的全体数据
  - 只有一个
- **外模式**
  - 是面向用户的数据需求视图
  - 模式的子集
  - 可以有很多个
- **内模式**
  - 是整个数据库的底层表示
  - 只有一个

### 模式映像

1. 外模式/模式映像

   模式描述的是全局的逻辑结构，外模式描述的是数据的局部逻辑结构

2. 模式/内模式映像

   定义了数据库的逻辑结构与物理存储之间的对应关系

## 第三章--关系数据库

关系模型三要素：关系模型的数据结构、关系模型的操作集合、关系模型的完整约束性

### 关系数据模型

- 数据结构：表
- **数据操作——操作的是表**
  - 并交差乘
  - 选择、投射、连接、除
  - 查询、插入、删除、更改
- 数据完整性约束
  - 与现实世界中应用需求的数据的相容性和正确性
  - 数据库内数据的相容性和正确性

### 基本术语

- 关系

- 属性

- 值域

- 元组：二维表的一行数据称为一个元组

- 分量：元组中的每一个属性值称为元组的一个分量

- 关系模式：二维表的结构称为关系模式

  设有关系名$R$，属性分别为${A_1,A_2,...,A_n}$，则关系模式表示为：$R({A_1,A_2,...,A_n})$

  每个$A_i(i=1,...,n)$还包括该属性到值域的映射，及属性的取值范围

  例如：学生（学号，姓名，年龄，性别，所在系）

- 关系数据库：对应于一个关系模型所有关系的集合

- 候选键：如果一个属性或属性集的值能够唯一标识一个关系的元组而有不包含多余的属性，则这个属性或属性集为候选键

  例如：学生（学号，姓名，年龄，性别，所在系）的候选键为学号

- **主键:当一个关系中有多个候选键，可以从中选一个做主键**

- 主属性和非主属性：包含在任意候选键的属性称为主属性，不包含在任意候选键的属性称为非主属性

### 形式化定义

1. 关系的形式化定义

   设$D_1,D_2,...,D_n$为任意集合，定义笛卡尔积$D_1,D_2,...,D_n$为：

   ​	$${D_1\times D_2\times ...\times D_n=\{(d_1,d_2,...,d_n)|d_i\in D_i,i=1,2,...,n\}}$$

   其中每一个元素$(d_1,d_2,...,d_n)$称为一个$n$元组，简称元组。元组中的每一个$d_i$称为一个变量

   例如：

   ![58f62c5f7d58828ea2580bbd1652f610](1.jpg)

2. 对关系的限定

   - 关系的每个分量必须是不可再分的最小属性
   - 表中数据是固定的
   - 不同列的数据可以取相同的值域
   - 关系表中的列顺序不重要
   - 关系表中的行的顺序也不重要
   - 同一关系中的元组不能重复

### 完整性约束

#### 实体完整性

指关系数据库中所有的表都必须有主键，且表中不允许存在以下记录:

- 无主键值的记录
- 主键值相同的记录

#### 参照完整性

描述实体之间的关系，实体之间既可以不同的实体，也可以是同一个实体

例1：

学生（<u>学号</u>，姓名，性别，专业，年龄）

课程（<u>课程号</u>，课程名，学分）

选课（<u>学号</u>，<u>课程号</u>，成绩）

“选课”关系中的“学号”引用了“学生”关系中的主键“学号”，“选课”关系中的“课程号”引用了“课程”关系中的主键“课程号”

例2：

有关系模式：职工（<u>职工号</u>，姓名，性别，直接领导职工号）

“职工号”是主键，“直接领导职工号”的取值参照了该关系中“职工号”属性的取值

**外键：设F是关系R的一个或一组属性，如果F与关系S的主键相对应，则称F是关系S的外键**

- 或者值为空
- 或者等于其所参照的关系中的某个元组的主键值

### 关系代数

#### 传统的集合运算

交并差积

#### 专门的关系运算

有这么三个关系：

| Student | Sno（学号）   | Sname(姓名)   | Ssex(性别)   | Sage(年龄)         | Sdept（所在系）  |      |
| ------- | ------------- | ------------- | ------------ | ------------------ | ---------------- | ---- |
| Course  | Cno（课程号） | Cname(课程名) | Credit(学分) | Semester(开课学期) | Pcno(直接先修课) |      |
| SC      | Sno（学号）   | Cno（课程号） | Grade(成绩)  |                    |                  |      |

#### 选择

选择运算符表示为：

$$\sigma_F(R)=\{t| t\in R \land F(t)='真'\}$$

其中，$\sigma$是选择运算符，R是关系名，t是元组，F是逻辑表达式，取逻辑“真”值或"假"值

例：选择计算机系学生信息的关系代数表达式为：

$\sigma_{Sdept='计算机'}(Student)$

#### 投影

从关系R中选取若干个属性，并将这些属性组成一个新的关系。

投影运算表示为:

$\prod_A(R)=\{t.A|t\in R\}$

其中，$\prod$是投影运算符，R是关系名，A是被投影的属性或属性组。t.A 表示t这个元组中相应属于属性（集）A的分量

投影一般有两个步骤：

1. 选出指定的属性，形成一个可能含有重复的新关系
2. 删除重复项，形成结果关系

例：对学生关系，在Sname，Sdept两个列上进行投影运算，可以表示为

$\prod_{Sname,Sdept}(Student)$

#### 连接

用来连接相互之间有关系的两个关系，从而产生一个新的关系。

$\theta$连接运算的一般表示为：

$R\underset{A\theta B}\Join S=\{t_r\land t_s |t_r\in R\land t_s\in S \land t_r[A]\theta t_s[B] \}$

其中,A和B分别是关系R和S上语义相同的属性或属性组，$\theta $是比较运算符。连接运算从R和S的广义笛卡尔积$R \times S$中选择关系（R关系）在A属性组上的值与（S属性）在B属性组上值满足比较运算关系符$\theta $的元组

- **等值连接**

  $R \underset{A=B} \Join S=\{t_r \land t_s| t_r \in R \land t_s \in S \land t_r[A]=t_s[B]\}$

  从R与S的广义笛卡尔积中选取A、B属性值相等的那些元组

- **自然连接**

  $R \Join S=\{t_r \land t_s| t_r \in R \land t_s \in S \land t_r[A]=t_s[B]\}$

  在**等值连接**的基础上去掉重复的属性列，使公共属性列值保留一个，**要求R与S之间必须有公共的属性名**

- **外连接**

  - 左外连接：$R^*\Join S$

    把R中不满足连接条件的元组保留到连接结果的后面

  - 右外连接：$R\Join ^*S$

    把S中不满足连接条件的元组保留到连接结果的后面

  - 全连接：$R^*\Join ^*S$把R和S中不满足连接条件的元组保留到连接结果的后面

#### 除

[数据库-——关系代数的除法运算最白话解析_数据库除法运算-CSDN博客](https://blog.csdn.net/Imomoco/article/details/102727039)

自己去看吧，打字好累

#### 总结案例

例题7：查询选了C002课程的学生的学号和成绩

$\prod_{Sno,Grade} (\sigma_{Cno='C002'}(SC))$

例题8：查询信息管理系选了C004课程的学生的姓名和成绩

$\prod_{Sname,Grade}(\sigma_{Cno='C004'\land Sdept='信息管理系'}(SC\Join Student))$

也可以这样写：

$\prod_{Sname,Grade}(\sigma_{Cno='C004'}(SC)\Join \sigma_{ Sdept='信息管理系'}(Student))$

这种写法执行效率更高

例题9：查询选了第二学期开设的课程的学生姓名、所在系和所选的课程号

$\prod _{Sname,Sdept,Cno}(\sigma_{Semester='第二学期'}(Student\Join SC \Join Course))$

为什么要用三个一起$\Join$，因为在SC里主键是Cno和Sno一起组成的

例10：查询选了“高等代数”且成绩大于等于90分的学生姓名、所在系和成绩

$\prod _{Sname,Sdept,Grade}(\sigma_{Grade>=90\land Cno='高等代数'}(Student\Join SC \Join Course))$

例11：查询没选VB课程的学生的姓名和所在系

$\prod _{Sname,Sdept}-\prod _{Sname,Sdept}(\sigma_{Cname='VB课程'}(Student\Join SC \Join Course))$

例12：查询选了全部课程的学生的姓名和所在系

查询选了全部课程的学生可以这样写：

$\prod _{Sno,Cno}(SC) \div \prod _{Cno}(Course)$

再从得到的Sno集合里找Sname,Sdept

$\prod _{Sname,Sdept}(Student \Join (\prod _{Sno,Cno}(SC) \div \prod _{Cno}(Course)))$

例12：查询计算机系选了第1学期开设的全部课程的学生的学号和姓名

懒得写了

## 第四章--SQL及数据定义功能

### SQL

| SQL功能  |         动词         |
| :------: | :------------------: |
| 数据定义 |   CREAT,DROP,ALTER   |
| 数据查询 |        SELECT        |
| 数据更改 | INSERT,UPDATE,DELETE |
| 数据控制 |  GRANT,REVOKE,DENY   |

### SQL支持的数据类型

#### 数值型

##### 准确型

| 数据类型 |  说明  | 存储空间 |
| :------: | :----: | :------: |
|   Int    |        |  4字节   |
| Tinyint  | 0到255 |  2字节   |
|   Bit    |  0或1  |  1字节   |
|  Bigint  |        |  8字节   |

##### 近似型

| 数据类型 | 说明 | 存储空间 |
| :------: | :--: | :------: |
|  float   |      |  4字节   |

#### 字符串类型

|   数据类型   |        说明         |         存储空间         |
| :----------: | :-----------------: | :----------------------: |
|   char(n)    |                     |          n字节           |
|  varchar(n)  | n表示字符串最大长度 | 字符数，1个汉字算2个字符 |
| varchar(max) |       大文本        |                          |
|   nchar(n)   |     字符串类型      |         2*n字节          |

#### 日期时间类型

| 数据类型  |                          说明                          |        存储空间         |
| :-------: | :----------------------------------------------------: | :---------------------: |
|   date    | 定义一个日期，范围为0001-01-01到9999-12-31，YYYY-MM-DD |          3字节          |
| time[(n)] |              24小时制，hh:mm:ss[.nnnnnn]               |        3到5字节         |
| datetime  |                 YYYY-MM-DDhh:mm:ss.nnn                 | 8字节（mysql只有6字节） |

### 数据定义功能

| 对象 |     创建      |    修改     |    功能     |
| :--: | :-----------: | :---------: | :---------: |
| 架构 | CREATE SCHEMA |             | DROP SCHEMA |
|  表  | CREATE TABLE  | ALTER TABLE | DROP TABLE  |
| 视图 |  CREATE VIEW  | ALTER VIEW  |  DROP VIEW  |
| 索引 | CREATE INDEX  | ALTER INDEX | DROP INDEX  |

#### 架构的定义与删除

架构（又称模式）:数据库下的一个逻辑命名空间，他是数据库对象的容器

创建架构

``` sql
CREATE SCHEMA { <架构名> 
	| AUTHORIZATION <所有者名>
	| <架构名> AUTHORIZATION <所有者名>
}
[{表定义预计 | 视图定义语句 | 授权语句 | 拒绝权限语句 }][;]
# 执行架构语句的用户必须具有数据库管理员权限，
# 或者是获得了数据库管理员授予的 CREATE SCHEMA 的权限

# 为用户 “HMT” 定义一个架构，架构名为 “S_C”
CREATE SCHEMA S_C AUTHORIZATION HMT
```

修改架构

``` sql
# 架构修改使用 ALTER SCHEMA 语句，可用于在同一数据库中的架构之间移动安全对象
ALTER SCHEMA <架构名>
TRANSFER <对象名> [;]

# 将表 Address 从架构 Person 传输到 HumanResources 架构
ALTER SCHEMA HumanResources 
TRANSFER Person.Address
```

删除架构

``` sql
DROP SCHEMA <架构名> [;]

# 删除S_C 架构
DROP SCHEMA S_C
```

#### 数据库

创建

``` sql
CREATE DATABASE <数据库名>
[ON [PRIMARY] <文件> [, ...n]
	[, <文件组> [, ...n]]
	[ LOG ON <文件> [, ...n]]]
[COLLATE <校验方式名>]
[WITH <选项> [, ...n]][;]

# 创建一个StudentCourse数据库
CREATE DATABASE StudentCourse
```

修改

``` sql
ALTER DATABASE <数据库名>
	ADD FILE <文件名> [, ...n]
		[ TO FILEGROUP {文件组} ]
	| ADD LOG FILE <文件> [, ...n]
	| REMOVE FILE <文件> [, ...n]
	| MODIFY FILE <文件> [, ...n]

/*
将一个大小为10M的数据文件  student_Data.mdf 添加到 StudentCourse 数据库中，
该文件的大小为10M，最大的文件大小为100M，增长速度为2MB，物理地址为D盘
*/
ALTER DATABASE StudentCourse
ADD FILE
(
	Name=student_Data,
    FILENAME='D:\sql\student_Data.mdf',
    Size=100M,
    Maxsize=100MB,
    Filegrowth=2MB
)
```

删除

``` sql
DROP DATABASE <数据库名> [, ...n][;]

# 将 StudentCourse 数据库删除
DROP DATABASE StudentCourse
```

#### 表

创建表

``` sql
CREATE TABLE [<架构名>.]<表名>
(
	{<列名> <数据类型> [列级完整性约束定义 [, ...n]]}
    [表级完整性约束 ][, ...n]
)
```

- NOT NULL：非空约束，限制列取值非空。
- PRIMARY KEY：主键约束，指定本列为主键。
- FOREIGN KEY：外建约束，定义本列为引用其他表的外建。
- UNIQUE：唯一值约束，限制列取值不能重复。
- DEFAULT：默认值约束，指定列的默认值。
- CHECK：列取值范围约束，限制列的取值范围。

多个属性列的约束必须在“**表完整性约束定义**”处定义，CHECK限制的列必须在同一个中

例：**使用SQL语句创建一个Student表**

``` sql
CREATE TABLE Student
(
	Sno CHAR(7) PRIMARY KEY,
    Sname NCHAR(20) NOT NULL,
    Ssex NCHAR(2) NOT NULL DEFAULT '男' CHECK (Ssex in ('男', '女')),
    Sbirthday DATETIME,
    Sdept NVARCHAR(20)
)
```

修改表

``` sql
ALTER TABLE [<架构名>.]<表名>
{
	ALTER COLUMN <列名> <新数据类型> -- 修改表定义
	| ADD <列名> <数据类型> [约束] -- 添加新列
	| DROP COLUMN <列名> -- 删除列
	| ADD [CONSTRAINT <约束名>] 约束定义 -- 添加约束
	| DROP <约束名> -- 删除约束
}

-- 为Student表添加备注(Memo)列，此列名为Memo，数据类型为text
ALTER TABLE Student
	ADD Memo text
	
-- 将Student表的Sname列的数据类型修改为 NVARCHAR(40)
ALTER TABLE Student
	ALTER COLUMN Sname NVARCHAR(40) -- MySQL用的是 MODIFY 而不是 ALTER
```

删除表

``` sql
DROP TABLE <表名> 

-- 删除Student表
DROP TABLE Student
```

以上参考于[^2]，阿里嘎多

## 第五章--数据操作语句

### 查询功能（SELECT）

``` sql
SELECT <目标列名序列>
	FROM <表名> [JOIN <表名> ON <连接条件>]
	[WHERE <行选择条件>]
	[GROUP BY <分组依据列>]
	[HAVING <组选择条件>]
	[ORDER BY <排序依据项>]
```

#### 选择表中若干列

``` sql
SELECT * FROM Student
SELECT Sno,Sname FROM Student
SELECT Sname,2015-Sage FROM Student
```

指定列名别名的语法：

``` sql
列名|表达式 [AS] 列别名
SELECT Sname 姓名,2015-Sage 年份 FROM Student
```

#### 选择表中若干元组

``` sql
SELECT DISTINCT Sno From SC
```

`DISTINCT`可以去掉重复行

#### WHERE

|  查询条件  |                 谓词                 |
| :--------: | :----------------------------------: |
| 比较运算符 |             =、>、>=...              |
|  确定范围  | BETWEEN ... AND、NOT BETWEEN ... AND |
|  确定集合  |              IN、NOT IN              |
|  字符匹配  |            LIKE、NOT LIKE            |
|  多重条件  |               AND、OR                |
|    空值    |         IS NULL、IS NOT NULL         |

``` sql
SELECT Sname FROM Student
WHERE Sdept='计算机'

SELECT Sname,Sdept,Sage FROM Student 
WHERE Sage BETWEEN 20 AND 23
等价于
SELECT Sname,Sdept,Sage FROM Student 
WHERE Sage>=20 AND Sage<=23

SELECT Sname,Sdept,Sage FROM Student 
WHERE Sage NOT BETWEEN 20 AND 23

SELECT Sname,Sage FROM Student 
WHERE Sdept IN ('信息管理系','通信工程'，'计算机系')
```

`LIKE`的匹配串中有四种通配符：

- _：匹配任何一个字符
- %：匹配0到多个字符
- \[ ]:匹配\[ ]里的任意一个字符。如[acdg]表示匹配a,c,d,g中的任何一个。假如匹配的字符是连续的，也可以用‘-’表达，比如[bcdge]可以写作[b-e]
- \[\^]:不匹配[]中的任意一个字符。假如匹配的字符是连续的，也可以用‘-’表达，比如\[\^bcdge]可以写作[\^b-e]

``` sql
SELECT * FROM Student WHERE Sname LIKE '张%'
SELECT * FROM Student WHERE Sname LIKE '张__'
SELECT * FROM Student 
WHERE Sname LIKE '[张李赵]%'
#就是查找有没有姓张或者姓李或者姓赵的学生
```

如果恰好找的字符串有通配符，如下划线或百分号，那就需要用到**ESCAPE**,表明位于那个字符后面的字符将被视为普通字符

``` sql
ESCAPE 转义字符
WHERE field1 LIKE '%30!%%' ESCAPE '!'
#像这个就是查找'30%'这个字符
```

**IS NULL**是个非常有意思的问题，可以看这个[^3]，其实我没找到讲究看吧

#### 对查询结果排序

``` sql
ORDER BY <列名> [ASC|DESC] [,...,n]
```

ASC代表升序，DESC代表降序，默认ASC

``` sql
SELECT Sno,Grade FROM SC
WHERE Cno='C002'
ORDER BY Grade DESC
```

#### 使用聚合函数汇总数据

- `COUNT（*）`：统计表中元组个数
- `COUNT([DISTINCT]<列名>)`：统计列值个数，DISTINCT选项表示去掉列的重复值后再统计
- `SUM(<列名>)`：计算列的和值
- `AVG(<列名>)`：计算列的均值
- `MAX(<列名>)`：得到列的最大值
- `MIN(<列名>)`：得到列的最小值

除了`COUNT（*）`以外，其他函数均忽略NULL值

``` sql
SELECT COUNT(*) FROM Student
SELECT COUNT(DISTINCT Sno) FROM SC
```

#### 对数据进行分组统计

``` sql
GROUP BY <分组依据列> [,... n]
[HAVING <组提取条件>]
```

`GROUP BY`提供对数据分组的功能，`HAVING`提供对分组后统计结果再筛选

``` sql
SELECT Cno as 课程名，COUNT(Sno) as 选课人数
FROM SC GROUP BY Cno

SELECT Sno,COUNT(*) 选课人数 FROM SC
GROUP BY Sno HAVING COUNT(*)>3
#选择出选课门数大于3的学生的学号
```

- WHERE用来筛选FROM的数据源的行数据
- GROUP BY用来对WHERE筛选后的结果数据进行分组
- HAVING用来对分组后的统计结果进行再筛选

正确的：

``` sql
SELECT Sdept,COUNT(*) FROM Student
	WHERE Sage<=20
	GROUP BY Sdept
```

错误的：

``` sql
SELECT Sdept,COUNT(*) FROM Student
	GROUP BY Sdept
	HAVING Sage <=20
```

因为在这个情况下，GROUP BY已经把数据先给筛选出来只剩Sdept和COUNT(*)了，HAVING不能对Sage进行操作

### 多表连接查询

#### 内连接

ANSI连接语法

``` sql
FROM 表1 [INNER] JOIN 表2 ON <连接条件>
<连接条件>:[<表名1>.][<列名1>]<比较运算符>[<表名2>.][<列名2>]
```

比较运算符为'='时，就是等值连接

``` sql
SELECT * FROM Student INNER JOIN SC
	ON Student.Sno = SC.Sno
```

当用'*'时，多表连接后可能存在重复项得改成一下这样

``` sql
SELECT Student.Sno,Sname,Ssex,Sage,Sdept,Cno,Grade 
	FROM Student JOIN SC
	ON Student.Sno = SC.Sno 
```

为表指定别名

``` sql 
表名|表达式 [AS] 表别名
```

**当为表指定别名后，在后面查询中，所有用到该表名的都必须用别名**

``` sql
SELECT Sname,Cname,Grade
	FROM Student s JOIN SC ON s.Sno=SC.Sno
	JOIN Course c ON c.Cno=SC.Cno
	WHERE Sdept ='信息管理系' AND Cname='计算机文化学'
```

#### 自连接

是一种特殊的内接表，是指相互连接的表在物理上为同一张表，但在逻辑上看为两张表

``` sql
FROM 表1 AS T1 
JOIN 表1 AS T2
ON 表1.列名=表2.列名
```

使用自连接的时候一定要为表取别名

例47：查询与刘晨在同一个系学习的学生姓名和所在系

``` sql
SELECT Sname,Sdept FROM Student AS s1
JOIN Student AS s2 ON s1.Sdept=s2.Sdept
WHERE s1.Sname='刘晨' AND s2.Sname!='刘晨'
```

#### 外连接

``` sql
FROM 表1 LEFT|RIGHT [OUTER] JOIN 表2 ON <连接条件>
```

`LEFT [OUTER] JOIN`这是左外连接，`RIGHT [OUTER] JOIN`这是右外连接

例49：查询全体学生的选课情况，包括选了课的学生和没有选课的学生

``` sql
SELECT Student.Sno,Sname,Cno,Grade
	FROM Student S
	LEFT JOIN SC ON S.Sno=SC.Sno
```

例50：查询没有人选的课程的课程名

``` sql
SELECT Course.Cname FROM Course c
LEFT JOIN SC ON C.Cno=SC.Cno
WHERE SC.Cno IS NULL
```

例52：统计计算机系每个学生的选课门数，包括没选课的学生

``` sql
SELECT Student.Sname,Sno COUNT(SC.Cno) 
FROM Student S LEFT JOIN SC ON S.Sno=SC.Sno
WHERE Sdept='计算机系'
GROUP BY S.Sno
```

做的好烂啊

### TOP

``` sql
TOP n [percent] [WITH TIES]
```

- n为非负整数
- TOP n：取查询结果的前n行数据
- TOP n percent：取查询结果的前n%行数据
- WITH TIES：包括并列结果

例54：查询年龄最大的三个学生的姓名、年龄及所在系

``` sql
SELECT TOP 3 [WITH TIES] Sname,Sage,Sdept
	FROM Student
	ORDER BY Sage DESC 
```

### CASE

简单CASE表达式

``` sql
CASE 测试表达式
	WHEN 简单表达式1 THEN 结果表达式1
	WHEN 简单表达式2 THEN 结果表达式2
	...
	WHEN 简单表达式n THEN 结果表达式n
	[ELSE 结果表达式n+1]
END
```

简单表达式可以是一个变量名、字段名、函数或子查询

简单表达式不能包含比较运算符

例58:

``` sql
SELECT Student.Sno,Sname,
	CASE Sdept
		WHEN '计算机系' THEN 'CS'
		WHEN '信息管理系统' THEN 'IM'
		WHEN '通信工程' THEN 'COM'
	END,Grade
	FROM Student S JOIN SC ON S.Sno=SC.Sno
	JOIN Course C ON C.Cno=SC.Cno
WHERE Cname='VB'
```

搜索CASE表达式

``` sql
CASE
	WHEN 布尔表达式1 THEN 结果表达式1
	WHEN 布尔表达式2 THEN 结果表达式2
	...
	WHEN 布尔表达式n THEN 结果表达式n
	[ELSE 结果表达式n+1]
END
```

布尔表达式可以用比较运算符、逻辑运算符

### 将查询的结果保存到表中

``` sql
SELECT 查询列表序列 INTO <新表名>
	FROM 数据源
	...
```

例62：

``` sql
SELECT Sno,Sname,Ssex,Sage
INTO Student_CS
#插入一个新的Student_CS表
FROM Student WHERE Sdept='计算机系'
```

- 局部临时表：表名前加#，只能在创建词局部临时表的当前连接中使用
- 全局临时表：表名前加##，能被所有的连接使用

例67：统计每个计算机系学生的选课门数，包括没有选课的学生，并将结果保存到局部临时表#CS_Sno中

``` sql
SELECT Student.Sno,Sname COUNT(SC.Cno)
INTO #CS_Sno
FROM Student S LEFT JOIN SC ON S.Sno=SC.Sno
WHERE S.Sdept='计算机系'
GROUP BY S.Sno
```

### 子查询

吃完再来









------

[^1]: https://blog.csdn.net/ttzax2015/article/details/81058251)

[^2]:https://blog.csdn.net/qq_42464569/article/details/112027778
[^3]:https://www.begtut.com/mysql/mysql-is-null.html
