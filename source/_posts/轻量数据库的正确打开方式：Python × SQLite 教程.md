---
title: 轻量数据库的正确打开方式：Python x SQLite 教程
date: 2025-06-09
categories:
- 编程语言
tags:
- Python
- SQLite
- 数据库
excerpt: "用 Python 演示如何使用 SQLite，从建库、建表到基本增删查改的完整流程"
---

&nbsp;

## 0.为什么选择 Python + SQLite?

在开发一些轻量级的项目时，相比起安装 MySQL、PostgreSQL 等大型数据库系统，SQLite 提供了一个“零配置、文件级、嵌入式”的解决方案 —— 只需要一个 `.db` 文件就能开始操作数据库。

而在Python内置的`sqlite3` 模块让操作sqlite变得非常简单，无需额外以来就能执行SQL语句，管理表结构。

接下来我们就一步一步通过实际代码来看看，如何用 Python 玩转 SQLite。

## 1.连接数据库与创建数据库文件

### 文件数据库

```python
import sqlite3
conn = sqlite3.connect("data.db")
```

- `connect()`：本地创建或者打开数据库文件，参数为数据库文件路径

- `conn`：是**连接对象**，可以理解为数据库的会话窗口，通过conn可以：执行SQL，提交事务，回滚，关闭连接

## 内存数据库

```python
import sqlite3
conn = sqlite3.connect(":memory:")
```

当`connect()`的参数为`:memory:`，sqlite将不会创建文件而是将文件存储在内存中，程序退出后数据消失，因为没有IO操作所以速度更快，一般用于程序测试时

## 2.执行SQL命令

### SQL 语句提交函数

```python
cursor = conn.cursor()
cursor.execute("INSERT INTO users (name) VALUES (?)", ("Bob",))
conn.commit()
```

- `conn.cursor()`：返回一个*游标对象*，cursor扮演者一个执行SQL命令+获取查询结果的中间人角色

- `cursor.execute()`：提交SQL命令

- `conn.commit()`：**提交事务**，把你对数据库的所有改动**永久写入数据库文件**

### SQL预处理

#### 问号占位符

```python
cursor.execute("SELECT * FROM users WHERE name = ?", ("Alice",))
```

- 通过?的方式占位，参数必须为元组或者列表，具有自动转义和类型处理机制，有效防御SQLi

#### 命名占位符

```python
 cursor.execute("SELECT * FROM users WHERE name = :name", {"name": "Alice"})
```

- 命名占位符有更高的可读性

- 使用 `:参数名` 的方式

- 参数用字典传入

- 适合参数多、复杂时使用

### 非预期处理方式（风险写法❌）

```python
name = "Alice"
cursor.execute(f"SELECT * FROM users WHERE name = '{name}'")
```

字符串拼接的写法会导致SQL注入漏洞

### 3.批量执行：executemany 的使用

```python
data = [("Tom",), ("Jerry",), ("Spike",)]
cursor.executemany("INSERT INTO users (name) VALUES (?)", data)
conn.commit()
```

- `executemany()` 用于一次性插入多行数据

- 避免使用 `for` 循环+多次 `execute()`，更高效，更优雅

### 4. 获取查询结果

#### 返回结果的三种函数

```py
cursor.execute("SELECT * FROM users")
rows = cursor.fetchall()
rows = cursor.fetchmany(n)
rows = cursor.fetchall()
conn.close()
```

- `fetchone()`：只获取一行

- `fetchmany(n)`：最多获取 n 行

- `fetchall()`：取所有结果，通常配合 for 使用

#### 查询结果作为字典

```python
conn.row_factory = sqlite3.Row
cursor = conn.cursor()
cursor.execute("SELECT * FROM users")
row = cursor.fetchone()
print(row["name"])  # 像访问字典一样取字段值
```

在默认情况下，SQLite 查询返回的是**元组形式的结果**

虽然能用，但你必须记得字段的顺序（`row[1]` 是 `name`），**代码可读性和健壮性都不高**。

设置`conn.row_factory = sqlite3.Row` 之后再执行查询时，返回的每一行就变成了一个**可以通过字段名访问的对象**

`sqlite3.Row`实际上是一个特殊对象，支持`row["字段名"]`

## 5.with自动管理连接

`sqlite3.connect()`返回的连接对象实现了上下文管理协议（即内部有`__enter__` 和 `__exit__` 方法），因此可以用 `with` 语法自动管理连接的生命周期。

传统写法`conn.close()`可能会导致数据未提交，文件锁未释放，内存泄漏等问题，而用`with`则可以自动调用`conn.commit()`和`conn.close()`

```python
with sqlite3.connect("data.db") as conn:
    cursor = conn.cursor()
    cursor.execute("SELECT * FROM users")
    rows = cursor.fetchall()
```

#### 背后的原理

Python 的 `with` 语句本质是调用对象的这两个方法：

- `__enter__()`：进入时返回连接对象

- `__exit__()`：退出时自动：
  
  - 提交事务（如果没有出错）
  
  - 或自动回滚（如果有异常）
  
  - 然后关闭连接

SQLite 的连接对象内置了这个行为。

## 6.总结

在本篇博客中，我们通过实际代码，介绍了如何用 Python 内置的 `sqlite3` 模块高效地操作 SQLite 数据库。你已经掌握了以下内容：

- 使用 `sqlite3.connect()` 创建数据库连接，包括**文件数据库**与**内存数据库**

- 使用 `conn.cursor()` 获取游标对象，执行 SQL 并获取查询结果

- 理解了 **SQL 预处理语句** 的作用与安全性（`?`、`:name`）

- 批量插入数据（`executemany`）的写法

- 多种结果读取方式（`fetchone()`、`fetchall()`、`row_factory`）

- 使用 `conn.commit()` 提交事务，确保数据持久化

- 利用 `with` 语法自动管理数据库连接，编写更安全的代码

SQLite 是轻量、无依赖、嵌入式的理想选择，而 Python 则提供了简洁优雅的操作接口。两者结合，几乎可以应付所有小型项目、自动化脚本、离线工具、甚至 Web 后端的原型开发。


