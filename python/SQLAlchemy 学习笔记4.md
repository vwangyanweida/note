# SQLAlchemy 学习笔记

- 原文 [SQLAlchemy架构中文翻译](http://nettee.github.io/posts/2016/SQLAlchemy-Translation/) 

## 1. SQLAlchemy 抽象方法

　SQLAlchemy称自己是一个工具包，强调开发人员的角色应是关系结构及其与应用程序间联系的设计者和构建者，而不是被动地接受一个第三方库所做的决定。SQLAlchemy采取“不完全抽象”理念，暴露关系概念，鼓励开发者在应用程序和关系数据库之间裁剪出一个自定义的、但又是完全自动化的交互层。SQLAlchemy的创新之处在于，它在不牺牲开发者对于关系数据库的控制的同时，实现了高度的自动化。

## 2. 核心层和ORM层

SQLAlchemy工具包的中心目标是在数据库交互的每一层都开放丰富的API，将任务划分为核心层和ORM层

1、 核心层

- Python数据库API(DBAPI)的交互 (python 数据库API接口是各数据库共有的一套接口）
- 生成数据库能够识别的SQL语句 （在这里翻译成sql)
- 模式(schema)管理

这些功能都通过公开的API来展现。

2、 ORM层或者叫对象-关系映射层

- 构建在核心层上的一个特定的库。

SQLAlchemy提供的ORM层只是可以构建在核心层上的众多对象抽象层的其中一个，很多开发者和组织是直接在核心层上构建自己的应用。

3、 SQLAlchemy 层次图

![关系图](https://raw.githubusercontent.com/nettee/SQLAlchemy-survey/master/picture/layers.png)

4、 核心层和ORM层的分离一直是SQLAlchemy最典型的特征，有优点又有缺点，导致

- ORM需要将映射到数据库的类属性关联到一个叫Table的结构上，而不是直接关联到数据库中表述的字符串属性名。

- ORM需要使用一个叫select的结构来产生SELECT查询，而不是直接将对象属性拼接成一个字符串的语句。

- ORM需要从ResultProxy接受结果行（ResultProxy自动将select映射到每个结果行），而不是直接操纵数据库游标(cursor)将数据转化成用户定义的对象。

　　因为核心层和ORM层的分离，导致ORM层的所有操作都是调用核心层实现的，而不是直接和数据库交互，所以ORM和数据库中间隔了一层，需要用核心层的数据结构来调用和沟通。

- 使用时可以只是用ORM层，只有非常复杂的情况下，可以"下潜"一两个抽象层次，更具体、更细致的处理数据库，直接调用核心层的API，但是随着SQLAlchemy日趋成熟，ORM层提供了越来越多的全面周到的模式，核心层的API在常规使用中已经很少出现了。

- 缺点是一个指令必须经过更多的步骤，使得运行缓慢，cpython可以通过用C代码替换来提高效率，PyPy通过JIT的内联和编译减少了长调用链的影响，SQLAlchemy 遗留的性能问题或许已经可以忽略，不需要使用C代码来代替。

## 3. 改良DBAPI

> SQLAlchemy的底层是一个通过DBAPI和数据库进行交互的系统。DBAPI本身不是一个实际的库，只是一个规范。因此，DBAPI有不同的实现，有的是针对特定的目标数据库，比如MySQL或PostgreSQL，有的是针对特定的非DBAPI数据库适配器，如ODBC和JDBC。
  
> DBAPI为SQLAlchemy提出了两点挑战。一点挑战是为DBAPI的基本使用模式提供一个易用且功能全面的界面（译注：facade，即对外提供简化过的接口），另一点挑战是应对极其多变的DBAPI具体实现和数据库引擎。

## 4. 方言系统

方言指的是不同数据库的sql语法的不同，如mysql何sql server的sql语法并不完全相同。

## 5. 处理DBAPI多变性

类似工厂模式一样，通过对不同的DBAPI和数据库，使用对应的子类。










