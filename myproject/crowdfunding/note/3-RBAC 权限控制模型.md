# RBAC简介

**基于角色的访问控制 (Role-based access control, RBAC)** 是一种安全功能，权限控制的**核心**是==用户通过角色与权限进行关联==。在RBAC 模型中，一个用户可以对应多个角色，一个角色拥有多个权限，权限具体定义用户可以做哪些事情。



## Why？为什么要进行权限控制

如果没有权限控制，系统的功能完全不设防，全部暴露在所有用户面前。用户登录以后可以使用系统中的所有功能。这是实际运行中不能接受的。
所以权限控制系统的目标就是管理用户行为，保护系统功能。

## What？什么是权限控制

“权限”=“权力”+“限制”

## How？如何进行权限控制

### ① 定义资源

资源就是系统中需要保护起来的功能。具体形式很多：**URL 地址、handler 方法、service 方法、页面元素**等等都可以定义为资源使用权限控制系统保护起来。

### ② 创建权限

一个功能复杂的项目会包含很多具体资源，成千上万都有可能。这么多资源逐个进行操作太麻烦了。为了简化操作，可以将相关的几个资源封装到一起，打包成一个**“权限”**同时分配给有需要的人。

![image-20220521150525883](https://raw.githubusercontent.com/xj-070/MarkDown/project/myproject/crowdfunding/image-20220521150525883.png)

![image-20220521150532810](https://raw.githubusercontent.com/xj-070/MarkDown/project/myproject/crowdfunding/image-20220521150532810.png)

### ③ 创建角色

对于一个庞大系统来说，一方面需要保护的资源非常多，另一方面操作系统的人也非常多。

**把资源打包为权限是对操作的简化**，同样**把用户划分为不同角色也是对操作的简化**。否则直接针对一个个用户进行管理就会很繁琐。
所以**角色就是用户的分组、分类**。先给角色分配权限，然后再把角色分配给用户，用户以这个角色的身份操作系统就享有角色对应的权限了。

### ④ 管理用户

系统中的用户其实是人操作系统时用来登录系统的**账号、密码**。

### ⑤ 建立关联关系（RBAC0）

**权限→资源**：单向多对多

- Java 类之间单向：
  - 从权限实体类可以获取到资源对象的集合，但是通过资源获取不到权限
- 数据库表之间多对多：
  - 一个权限可以包含多个资源
  - 一个资源可以被分配给多个不同权限

**角色→权限**：单向多对多

- Java 类之间单向：
  - 从角色实体类可以获取到权限对象的集合，但是通过权限获取不到角色
- 数据库表之间多对多：
  - 一个角色可以包含多个权限
  - 一个权限可以被分配给多个不同角色

**用户→角色**：双向多对多

- Java 类之间双向：可以通过用户获取它具备的角色，也可以看一个角色下包含哪些用户
- 数据库表之间：
  - 一个角色可以包含多个用户
  - 一个用户可以身兼数职



# 多对多关联关系在数据库中的表示

### 1.没有中间表

![image-20220521152334240](https://raw.githubusercontent.com/xj-070/MarkDown/project/myproject/crowdfunding/image-20220521152334240.png)

如果只能在一个**外键**列上存储关联关系数据，那么现在这个情况**无法**使用SQL 语句进行**关联查询**。（整个外键里的条件会被封装成一个字符串无法单独查询）

### 2.有中间表

![image-20220521152505868](https://raw.githubusercontent.com/xj-070/MarkDown/project/myproject/crowdfunding/image-20220521152505868.png)

select t_studet.id,t_student.name from t_student **left join** t_inner **on**  t_studen.id=t_inner.stuent_id **left join** t_subject **on** t_inner.subject_id=t_subject.id **where** t_subjct.id=1

**LEFT JOIN 关键字**会从**左表 (t_student ) 那里返回所有的行**，即使在**右表(t_inner)**中没有匹配的行。

### 3. 中间表的主键生成方式

#### ① 方式一：另外设置字段作为主键

![image-20220521153650775](https://raw.githubusercontent.com/xj-070/MarkDown/project/myproject/crowdfunding/image-20220521153650775.png)

#### ②方式二：使用联合主键

![image-20220521153709514](https://raw.githubusercontent.com/xj-070/MarkDown/project/myproject/crowdfunding/image-20220521153709514.png)

> 数据库中设置时都勾选主键，组合起来不能重复即可



# RBAC0~RBAC3

## 1. RBAC0

最基本的RBAC 模型，RBAC 模型的核心部分，后面三种升级版RBAC 模型也都是建立在RBAC0 的基础上。

## 2. RBAC1 

在RBAC0 的基础上增加了角色之间的**继承**关系。角色A 继承角色B 之后将具备B 的权限再增加自己独有的其他权限。

比如：付费会员角色继承普通会员角色，那么付费会员除了普通会员的权限外还具备浏览付费内容的权限。<img src="https://raw.githubusercontent.com/xj-070/MarkDown/project/myproject/crowdfunding/image-20220521155809027.png" alt="image-20220521155809027" style="zoom:50%;" />

## 3. RBAC2

在RBAC0 的基础上进一步增加了角色责任分离关系。责任分离关系包含**静态责任分离**和**动态责任分离**两部分。

- **静态责任分离**：给用户<font color = red>分配</font>角色时生效

  - **互斥角色**：

    权限上相互制约的两个或多个角色就是互斥角色。用户只能被分配到一组互斥角色中的**一个**角色。

    例如：一个用户不能既有会计师角色又有审计师角色。

  - **基数约束**：

    -  一个角色对应的**访问权限数量**应该是受限的

    - 一个角色中**用户的数量**应该是受限的

    - 一个用户拥有的**角色数量**应该是受限的

  - **先决条件角色**：

     用户想拥有A 角色就必须**先**拥有B 角色，从而保证用户拥有X 权限的前提是拥有Y 权限。

     例如：“金牌会员”角色只能授予拥有“银牌会员”角色的用户，不能**直接授予**普通用户。(进阶)

  - **动态责任分离**：

    用户登录系统时生效

    -  一个用户身兼数职，在**特定场景下激活**特定角色

      例如：张三在阿里巴巴内部激活创始人角色

    ​                   张三在某企业级论坛上激活演讲嘉宾角色


## 4. RBAC3

RBAC3 是在RBAC0 的基础上同时添加RBAC2 和RBAC3 的约束，最全面、最复杂。

<img src="https://raw.githubusercontent.com/xj-070/MarkDown/project/myproject/crowdfunding/image-20220521161314977.png" alt="image-20220521161314977" style="zoom:50%;" />

