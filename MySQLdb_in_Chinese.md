### Introduction 介绍

MySQLdb is an thread-compatible interface to the popular MySQL database server that provides the Python database API.

MySQLdb 是流行的Mysql 数据库的线程兼容接口，它提供了python的数据库API。


### Installation 安装

The README file has complete installation instructions.

README文件有完整的安装说明。


### _mysql

If you want to write applications which are portable across databases, use MySQLdb, and avoid using this module directly. _mysql provides an interface which mostly implements the MySQL C API. For more information, see the MySQL documentation. The documentation for this module is intentionally weak because you probably should use the higher-level MySQLdb module. If you really need it, use the standard MySQL docs and transliterate as necessary.

如果你想写一个app能轻松 跨多种数据库，用 MySQLdb，并避免直接使用_mysql库. _mysql 提供的接口 实现了大部分 Mysql的C语言API. 如需详情，自己看MySQL文档. 这篇关于 _mysql的文档故意弱化了，因为你可能更想用高层封装的 MySQLdb库。如果你真想用 _mysql，那你需要查看标准 MySQL文档 并 翻译。


### MySQL C API translation MySQL的C语言API翻译

The MySQL C API has been wrapped in an object-oriented way. The only MySQL data structures which are implemented are the MYSQL (database connection handle) and MYSQL_RES (result handle) types. In general, any function which takes MYSQL *mysql as an argument is now a method of the connection object, and any function which takes MYSQL_RES *result as an argument is a method of the result object. Functions requiring none of the MySQL data structures are implemented as functions in the module. Functions requiring one of the other MySQL data structures are generally not implemented. Deprecated functions are not implemented. In all cases, the mysql_ prefix is dropped from the name. Most of the conn methods listed are also available as MySQLdb Connection object methods. Their use is non-portable.


MySQL的C语言API 已经被封装成了面向对象。唯一实现了的MySQL 数据结构是 MYSQL(database connection handle)类型 和 MYSQL_RES(result handel)类型. 
总得来讲，
- 任何一个 接受 `MYSQL *mysql` 作为参数的function 现在都被实现成了 一个connecition object对象的method方法, 
- 并且 任何一个接受 `MYSQL_RES *result`作为参数的function 现在都被实现成了 一个result object对象的method.
- 那些需要MySQL 其他的数据结构的Functions 基本都没实现. 已过时的functions 没有实现. 总体上，前缀`mysql_`已经从名字中移除. 大多数列出的 conn methods 同样能在 MySQLdb connection object methods 中使用. 他们用起来不轻便(这句翻译的是个p).


> 你好，我是译者注释 k9，请发美女电话到 yan95207396@126.com
> 1. database connection handle(译者注: 就是连接mysql的方法, 用用就明白了)
> 2. result handel(译者注: 猜测是 把sql查询 转义出了 list, tuple之类的python数据结构)
> 3. 译者真是好人, 给了这么多注释 Orz. 我喜欢美女哈，邮箱 yan95207396@126.com. 1024.
> 4. MYSQL *mysql = int *p, 这是指mysql官方文档中. 这是译者准确地猜测，你要明白c指针. 如果猜错，欢迎指出 Orz.
> 5. conn methods 看下表


### MySQL C API function mapping 对照表
| C API | _mysql | Orz 注释
| :------------------- | --------:| --------: | 
|mysql_affected_rows() |	conn.affected_rows()| |
|mysql_autocommit()	| conn.autocommit()|
|mysql_character_set_name()	| conn.character_set_name()|
|mysql_close()	| conn.close()|
|mysql_commit()	| conn.commit()|
|mysql_connect() |	_mysql.connect()|
|mysql_data_seek() |	result.data_seek()|
|mysql_debug() |	_mysql.debug()|
|mysql_dump_debug_info |	conn.dump_debug_info()|
|mysql_escape_string() |	_mysql.escape_string()|
|mysql_fetch_row() |	result.fetch_row()|
|mysql_get_character_set_info() |	conn.get_character_set_info()|
|mysql_get_client_info() |	_mysql.get_client_info()|
|mysql_get_host_info() |	conn.get_host_info()|
|mysql_get_proto_info() |	conn.get_proto_info()||
|mysql_get_server_info() |	conn.get_server_info()|
|mysql_info() |	conn.info()|
|mysql_insert_id() |	conn.insert_id()|
|mysql_num_fields() | result.num_fields()|
|mysql_num_rows() |	result.num_rows()|
|mysql_options() |	various options to _mysql.connect()|
|mysql_ping() |	conn.ping()|
|mysql_query() |	conn.query()|
|mysql_real_connect() |	_mysql.connect()|
|mysql_real_query() |	conn.query()|
|mysql_real_escape_string() |	conn.escape_string()|
|mysql_rollback() |	conn.rollback()|
|mysql_row_seek() |	result.row_seek()|
|mysql_row_tell() |	result.row_tell()|
|mysql_select_db() |	conn.select_db()|
|mysql_set_character_set() |	conn.set_character_set()|
|mysql_ssl_set() | ssl option to _mysql.connect()|
|mysql_stat() |	conn.stat()|
|mysql_store_result() |	conn.store_result()|
|mysql_thread_id() |	conn.thread_id()|
|mysql_thread_safe_client() |	conn.thread_safe_client()|
|mysql_use_result() |	conn.use_result()|
|mysql_warning_count() | conn.warning_count()|
|CLIENT_* |	MySQLdb.constants.CLIENT.*|
|CR_* |	MySQLdb.constants.CR.*|
|ER_* |	MySQLdb.constants.ER.*|
|FIELD_TYPE_* |	MySQLdb.constants.FIELD_TYPE.*|
|FLAG_* |	MySQLdb.constants.FLAG.*|

 

### Some _mysql examples

Okay, so you want to use _mysql anyway. Here are some examples.
Okay, 如果你无论如何想用`_mysql`. 这是一些例子.

The simplest possible database connection is:
最简单的数据库连接:

``` python
import _mysql
db=_mysql.connect()
```
This creates a connection to the MySQL server running on the local machine using the standard UNIX socket (or named pipe on Windows), your login name (from the USER environment variable), no password, and does not USE a database. Chances are you need to supply more information.

在运行中的本机, 使用 标准 UNIX socket (or suck windows)创建了一个对MySQL 数据库的连接, 你的登录名(从用户环境变量), 没有密码, 并且不使用数据库. 你需要提供更多的连接信息：
``` python
db=_mysql.connect("localhost","joebob","moonpie","thangs")
```

This creates a connection to the MySQL server running on the local machine via a UNIX socket (or named pipe), the user name "joebob", the password "moonpie", and selects the initial database "thangs".

通过 UNIX socket (or suck windows) 创建了一个本地连接 MySQL 服务器的连接, 用户名 "joebob", 密码 "moonpie", 并且 使用了 "thangs"为初试数据库.


We haven't even begun to touch upon all the parameters connect() can take. For this reason, I prefer to use keyword parameters:

我们还没有接触到 `connect()`的所有参数. 基于此, 个人更倾向使用 关键字参数：

```python
# 推荐
db=_mysql.connect(host="localhost",user="joebob",
passwd="moonpie",db="thangs")
```

This does exactly what the last example did, but is arguably easier to read. But since the default host is "localhost", and if your login name really was "joebob", you could shorten it to this:
这个例子完全与上例功能相同, 但是, 可以说更易读. 但是因为 `host` 的默认值就是`localhost` , 并且，如果你的登陆名就是 `joebobo`, 你可以选择更简短的写法：

> 关键字参数 vs 位置参数, 自查

```python
# 译者不推荐， explicit is better than no-explicit.
db=_mysql.connect(passwd="moonpie",db="thangs")
```

UNIX sockets and named pipes don't work over a network, so if you specify a host other than localhost, TCP will be used, and you can specify an odd port if you need to (the default port is 3306):
UNIX sockets 和 suck windows 都不能通过网络工作, 如果你指明的不是`localhost`, TCP 协议会使用, 并且你可以指明监听接口, 如果你需要(默认值是3306)

> 1. 译者: localhost， 127.0.0.1, 本机, /etc/hosts
> 2. mysql默认监听3306, 呵呵，自己以前还真是傻呢

```python
# = =! Why is moonpie...? Is this from the big theory?
db=_mysql.connect(host="outhouse",port=3307,passwd="moonpie",db="thangs")
```

If you really had to, you could connect to the local host with TCP by specifying the full host name, or 127.0.0.1.
Generally speaking, putting passwords in your code is not such a good idea:
如果你真的需要，你可以通过指明全host name, 或者 127.0.0.1，就能用TCP连接本机了。
通常来说，把密码放到代码里，不是个好主意。译者：虽然基本我遇到的公司都是这样做，某企鹅的离职员工也没什么反应，setting文件权限也就是666.

```python
db=_mysql.connect(host="outhouse",db="thangs",read_default_file="~/.my.cnf")
```

This does what the previous example does, but gets the username and password and other parameters from ~/.my.cnf (UNIX-like systems). Read about option files for more details.

这与上例作用相同, 但是是通过 `~/.my.cnf` (类UNIX 系统) 获取用户名和密码以及其他参数. 阅读选择文件获得更多信息.

So now you have an open connection as db and want to do a query. Well, there are no cursors in MySQL, and no parameter substitution, so you have to pass a complete query string to db.query():
那么，现在你有了一个与db的连接，然后你想去执行查询. Well, 在MySQL 中没有 cursors的概念, 并且没有其替代的参数, 所以你不得不传一个完整的查询语句给 `db.query()`:

> 译者不明白, cursor在其他数据库中，是一个常有概念吗？很好用吗？ MySQLdb在抽象层面是实现了cursor的.

```python
db.query("""SELECT spam, eggs, sausage FROM breakfast
WHERE price < 5""")
```

There's no return value from this, but exceptions can be raised. The exceptions are defined in a separate module, _mysql_exceptions, but _mysql exports them. Read DB API specification PEP-249 to find out what they are, or you can use the catch-all MySQLError.

这个查询没有返回值, 但是可以抛出异常. 异常的定义在另外一个module库, `_mysql_exceptions`, 但是 `_mysql` 会加载他们. 阅读 DB API规范PEP-249能明白他们是什么, 或者你可以用`MySQLError` 去捕获所有异常.

> [PEP-249](http://legacy.python.org/dev/peps/pep-0249/), 要读啊，要读啊，不翻译也要读一读啊~

At this point your query has been executed and you need to get the results. You have two options:

目前你的查询已经被执行，然后你需要获得查询结果. 你有两个选择:

```python
r=db.store_result()
# ...or...
r=db.use_result()
```

Both methods return a result object. What's the difference? store_result() returns the entire result set to the client immediately. If your result set is really large, this could be a problem. One way around this is to add a LIMIT clause to your query, to limit the number of rows returned. The other is to use use_result(), which keeps the result set in the server and sends it row-by-row when you fetch. This does, however, tie up server resources, and it ties up the connection: You cannot do any more queries until you have fetched all the rows. Generally I recommend using store_result() unless your result set is really huge and you can't use LIMIT for some reason.

两个method 都会返回一个 result object. 有什么区别呢？ `store_result()` 立即返回 全部的结果集 给客户端. 如果你的结果集非常大, 这会引起问题. 关于此的一个解决方法是`LIMIT`约束, 以此限制你返回结果的数量. 另一个方法是用`user_result()`, 她能保存结果集在服务器端, 并且 在你`fetch` 获取结果的时候，一行一行地发送. 但是, 这会 绑定服务器资源, 并且 她会绑定连接: 你不能再做其他查询直到你`fetch`获取了所有行. 通常我推荐使用`store_result()` 除非你的结果集真的非常巨大 并且 你因为某些原因不能使用`LIMIT`.

Now, for actually getting real results:

现在, 真正地获取结果:

```python
>>> r.fetch_row()
(('3','2','0'),)
```

This might look a little odd. The first thing you should know is, fetch_row() takes some additional parameters. The first one is, how many rows (maxrows) should be returned. By default, it returns one row. It may return fewer rows than you asked for, but never more. If you set maxrows=0, it returns all rows of the result set. If you ever get an empty tuple back, you ran out of rows.

这可能看起来有点奇怪. 首先你要知道的是, `fetch_row()` 接受一些额外的参数. 第一个是, 多少行 (`maxrows` 最大行数). 默认值, 她会返回一行. 她可能会返回少于你要求的行数, 但是永远不会多于. 如果你设置 `maxrows=0`, 她返回结果集的所有行. 一旦你获得一个空元组回来, 你就遍历了所有行.

The second parameter (how) tells it how the row should be represented. By default, it is zero which means, return as a tuple. how=1 means, return it as a dictionary, where the keys are the column names, or table.column if there are two columns with the same name (say, from a join). how=2 means the same as how=1 except that the keys are always table.column; this is for compatibility with the old Mysqldb module.

第二个参数 (`how`) 决定了 行的展现方式. 
> 1. 默认值, `how` 是0, 这意味着返回值是 tuple元组. 
> 2. `how=1`, 返回值是 dict字典, key 键是 字段名(列名), 或者是 `table.column` 如果返回值有两个 相同的字段名(比如, 联合查询). 
> 3. `how=2`, 与`how=1` 基本相同, 除了 key 键总是 `table.column` 形式. 这是为了与旧的 MySQLdb库 兼容.



