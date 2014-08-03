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

 

OK, so why did we get a 1-tuple with a tuple inside? Because we implicitly asked for one row, since we didn't specify maxrows.

OK, 那么为什么我们获得一个 一维tuple在一个tuple中呢? 因为我们隐式要求返回一行, 因为我们没有指明`maxrows`参数.

The other oddity is: Assuming these are numeric columns, why are they returned as strings? Because MySQL returns all data as strings and expects you to convert it yourself. This would be a real pain in the ass, but in fact, _mysql can do this for you. (And MySQLdb does do this for you.) To have automatic type conversion done, you need to create a type converter dictionary, and pass this to connect() as the conv keyword parameter.

另外一个怪癖: 假设存在数字行, 为什么他们被返回成了字符呢? 因为MySQL 把所有的返回数据都当作字符串 并且 希望你靠自己所转化. 这真是**菊花痛啊**(`Orz`, 小的跪了, 这尼玛大神就是大神, 能让我翻译这样的话出来, 小的甚是荣幸啊 `T_T`), 但实际上, `_mysql` 可以帮你做这个. (并且 MySQLdb 确实会为你做这个.) 需要让 **自动转化类型执行**, 你需要创建一个 **类型转换 字典**, 并且 把这个 **类型转换 字典** 传入 `connect()` 作为 `conv` 转换关键字参数.

The keys of conv should be MySQL column types, which in the C API are FIELD_TYPE_*. You can get these values like this:

`conv`的 关键字 应该是MySQL 的 列类型, 列类型在 C API中是 `FIELD_TYPE_*`. 你能像这样获得返回值:

```python
from MySQLdb.constants import FIELD_TYPE
```

By default, any column type that can't be found in conv is returned as a string, which works for a lot of stuff. For our purposes, we probably want this:

默认, 如何不能从 `conv` 找到的 `列类型` 都作为一个`string`, 这试用与很多事情. 对我们而言, 我们可能想要这样:

```python
my_conv = { FIELD_TYPE.LONG: int }
```

This means, if it's a FIELD_TYPE_LONG, call the builtin int() function on it. Note that FIELD_TYPE_LONG is an INTEGER column, which corresponds to a C long, which is also the type used for a normal Python integer. But beware: If it's really an UNSIGNED INTEGER column, this could cause overflows. For this reason, MySQLdb actually uses long() to do the conversion. But we'll ignore this potential problem for now.

这意味着, 如果是一个 `FIELD_TYPE_LONG`类型, 会调用内置函数 `int()`做转换. 注意 `FIELD_TYPE_LONG`列 是一个 `INTEGER`整型列( **此处翻译准确吗？**), 与C语言中 long型相对应, 同时与python中的 `integer`整型相对应. 但要注意: 如果 列类型 是 `UNSIGNED INTEGER` 非负整型, 这可能会导致数据溢出. 基于此原因, MySQLdb真正使用 `long()`去做转换. 但是我们目前 会无视潜在问题.

Then if you use `db=_mysql.connect(conv=my_conv...)`, the results will come back `((3, 2, 0),)` , which is what you would expect.

然后, 如果你使用了 `db=_mysql.connect(conv=my_conv...)`, 返回值会是 `((3, 2, 0),)`, 这也是你想要的.

```python
>>> db=_mysql.connect(conv=my_conv...)
((3, 2, 0),)
```

### MySQLdb

MySQLdb is a thin Python wrapper around _mysql which makes it compatible with the Python DB API interface (version 2). In reality, a fair amount of the code which implements the API is in _mysql for the sake of efficiency.

MySQLdb 是对`_mysql`的一层简单封装, 这使她兼容 Python DB API interface(version 2). 实际上, 相当数量的代码为了效率, 使用`_mysql`实现了 API.

 

The DB API specification PEP-249 should be your primary guide for using this module. Only deviations from the spec and other database-dependent things will be documented here.

DB API规则**PEP-249**应该是你使用这个库的主要指导. **只有与规范不同 以及 其他的数据库依赖 会被记入这个文档. **

 

### Functions and attributes

Only a few top-level functions and attributes are defined within MySQLdb.

只有一些 top-level的 方法 和 属性在 MySQLdb 中被定义.

`connect(parameters...)`
Constructor for creating a connection to the database. Returns a Connection Object. Parameters are the same as for the MySQL C API. In addition, there are a few additional keywords that correspond to what you would pass mysql_options() before connecting. Note that some parameters must be specified as keyword arguments! The default value for each parameter is NULL or zero, as appropriate. Consult the MySQL documentation for more details. The important parameters are:

构造器为了 建立一个与数据库的连接. 返回一个 `Connection Object`连接对象. 参数与 MySQL C API相同. 此外, 有一些额外的关键字与传入`mysql_options()`的参数相对应, `mysql_options()` 在建立连接之前(Orz, 啥j8翻译...). 注意, 一些参数应该当作 **关键字参数**被赋值! 对每一个参数的默认值都是 NULL 或者 零, 如适用. 查询 MySQL 文档活得更多信息. 重要的参数有:

`host`
name of host to connect to. Default: use the local host via a UNIX socket (where applicable)
连接的主机名. 默认: 使用localhost通过一个 UNIX socket(如果可用)

`user`
user to authenticate as. Default: current effective user.
用户名用来授权. 默认: 当前的有效用户.

`passwd`
password to authenticate with. Default: no password.
用来授权的密码. 默认: 没有密码

`db`
database to use. Default: no default database.
要使用的 库. 默认: 不选择库.

`port`
TCP port of MySQL server. Default: standard port (3306).
MySQL服务器的TCP端口. 默认: 标准端口(3306)

`unix_socket`

location of UNIX socket. Default: use default location or TCP for remote hosts.

UNIX socket的路径. 默认：使用默认路径 或者 使用TCP连接远端。

> [? location of UNIX socket](http://stackoverflow.com/questions/7580346/where-to-place-unix-domain-af-unix-sockets-end-points-files#)
> This will help u to understand.

`conv`

type conversion dictionary. Default: a copy of MySQLdb.converters.conversions

定义一个 转化字典。 默认：`MySQLdb.converters.conversions`的拷贝。

`compress`

Enable protocol compression. Default: no compression.
开启压缩协议。 默认： 不开启。

`connect_timeout`

Abort if connect is not completed within given number of seconds. Default: no timeout (?)

忽略，如果在给定时间内超时。 默认：没有超时

`named_pipe`

Use a named pipe (Windows). Default: don't.

哦，windows。

`init_command`

Initial command to issue to server upon connection. Default: Nothing.

连接服务器时发出的初始化命令。 默认：空。

> ？连接mysql时的命令嘛？

`read_default_file`

MySQL configuration file to read; see the MySQL documentation for mysql_options().

MySQL 配置文件读取。请查阅 MySQL文档 中 `mysql_options()`

`read_default_group`

Default group to read; see the MySQL documentation for mysql_options().

同上。

> 暂不明。 需要查文档。

`cursorclass`

cursor class that cursor() uses, unless overridden. Default: MySQLdb.cursors.Cursor. This must be a keyword parameter.

光标类 被 `cursor()`方法使用，除非被重载了。 默认：`MySQLdb.cursors.Cursor`。这必须用** 关键字参数**

 

`use_unicode`

If True, CHAR and VARCHAR and TEXT columns are returned as Unicode strings, using the configured character set. It is best to set the default encoding in the server configuration, or client configuration (read with read_default_file). If you change the character set after connecting (MySQL-4.1 and later), you'll need to put the correct character set name in connection.charset.

如果`True`, `CHAR`和`VARCHAR`和`TEXT`的字段都会返回 Unicode字符串, 使用配置的字符集合. 最好在服务器配置中设置默认的编码，或者 客户端配置(从 `read_default_file`读). 如果在建立连接之后，你改变字符集(MySQL-4.1 或者 更新), 你需要将正确的 字符集名字 设置到 `connection.charset`.

If False, text-like columns are returned as normal strings, but you can always write Unicode strings.

如果`False`, 类text的字段会像往常返回普通字符串, 但是你依旧可以返回 Unicode字符串.

This must be a keyword parameter.

这只能当做**关键字参数**使用.

 
`charset`

If present, the connection character set will be changed to this character set, if they are not equal. Support for changing the character set requires MySQL-4.1 and later server; if the server is too old, UnsupportedError will be raised. This option implies use_unicode=True, but you can override this with use_unicode=False, though you probably shouldn't.

如果赋值, 连接的字符集会改变成这个字符集, 如果他们不一样. 支持改变字符集需要MySQL-4.1或者更高版本; 如果MySQL服务器版本过久, `UnsupportedError`异常会触发. 这个选项暗含着需要 `use_unicode=True`， 但是你可以选项使用 `use_unicode=False`, 尽管不应该这样用.

If not present, the default character set is used.

如果没有赋值, 默认字符集会被使用.

This must be a keyword parameter.

这应该是 **关键字参数**

`sql_mode`

If present, the session SQL mode will be set to the given string. For more information on sql_mode, see the MySQL documentation. Only available for 4.1 and newer servers.

如果设定，`session SQL 模式` 会设定成 给定的字符串的模式. 关于更多的`sql_mode`模式, 查阅MySQL文档. 只适合4.1 或更高版本.

> New skill. Get it:D [session SQL mode](http://dev.mysql.com/doc/refman/5.0/en/sql-mode.html)

If not present, the session SQL mode will be unchanged.

如果没有赋值, `session SQL 模式` 不会改变.

> Hi,  我现在已经能直接写markdown, 不需要预览了.
> 因为我的文档比较简单，这样简单的编辑就能满足我的想法了.
> 虽然我也总是忍不住希望使用 vim. 常常按Esc.

This must be a keyword parameter.

这必须是 **关键字参数**

`ssl`

This parameter takes a dictionary or mapping, where the keys are parameter names used by the mysql_ssl_set MySQL C API call. If this is set, it initiates an SSL connection to the server; if there is no SSL support in the client, an exception is raised. This must be a keyword parameter.

这个参数接收一个字典 或者 映射类型, 参数的键是 `mysql_ssl_set` MySQL C API调用的参数. 如果参数是一个集合, 她启动一个与服务器的SSL连接; 如果客户端不支持SSL, 会异常报警. 必须当做是 **关键字参数**.


`apilevel`

String constant stating the supported DB API level. '2.0'

字符串包括了支持 DB API等级2.0.

> 这是什么等级？

`threadsafety`

Integer constant stating the level of thread safety the interface supports. This is set to 1, which means: Threads may share the module.

整形声明了接口对线程安全的等级. 设置为1, 表明: 线程可能共享库.

The MySQL protocol can not handle multiple threads using the same connection at once. Some earlier versions of MySQLdb utilized locking to achieve a threadsafety of 2. While this is not terribly hard to accomplish using the standard Cursor class (which uses mysql_store_result()), it is complicated by SSCursor (which uses mysql_use_result(); with the latter you must ensure all the rows have been read before another query can be executed. It is further complicated by the addition of transactions, since transactions start when a cursor execute a query, but end when COMMIT or ROLLBACK is executed by the Connection object. Two threads simply cannot share a connection while a transaction is in progress, in addition to not being able to share it during query execution. This excessively complicated the code to the point where it just isn't worth it.

MySQL协议不能处理多个线程同时使用一个连接. 一些早期的MySQLdb利用锁来获得 2等级的线程安全. 虽然这样没有严重地影响使用标准的 `Cursor类`( Cursor类使用 `mysql_store_result()` ), 但是他们使用了`SSCursor`(使用 `mysql_use_result()`); 后者你必须保证所有行都被遍历, 在你执行下一次操作之前. 在之后的实现中使用了 事务, 因为事务开始于一个cursor执行查询, 但是当 `Connection类`执行`COMMIT` 或者 `ROLLBACK`时才结束. 只要当事务执行, 两个线程就能简单地不能共享连接, 并且在查询的时候也不能共享. 这不需要这样做的时候，这样过度复杂了代码.

> 译者: **`store_result()`推荐，配合`LIMIT`使用. 一次性返回所有结果集, 释放本次查询.
> `use_result()` 与此相反.

The general upshot of this is: Don't share connections between threads. It's really not worth your effort or mine, and in the end, will probably hurt performance, since the MySQL server runs a separate thread for each connection. You can certainly do things like cache connections in a pool, and give those connections to one thread at a time. If you let two threads use a connection simultaneously, the MySQL client library will probably upchuck and die. You have been warned.

通常的结论是: 不要在线程间共享连接. 这真的不值得你努力, 并且最终, 这样的行为可能会损害性能, 因为MySQL数据库为每一个连接都建立独立的线程. 你一定能做些像 建立线程池之类的事情, 并且把这些连接在一个时刻只给一个线程. 如果你让两个线程同时使用一个连接, MySQL客户端库很可能卡住 并 挂掉. 我已经警告过你了.

For threaded applications, try using a connection pool. This can be done using the Pool module.

为了线程应用, 试试用*连接池*. 这可以用`Pool 库`来实现.

`charset`

The character set used by the connection. In MySQL-4.1 and newer, it is possible (but not recommended) to change the connection's character set with an SQL statement. If you do this, you'll also need to change this attribute. Otherwise, you'll get encoding errors.

设置连接时使用的字符集. MySQL-4.1 或者 更高, (并不推荐)可以通过一个 SQL statement声明改变连接的字符集. 如果你这样做, 你同样需要改变这个属性. 否则,你会获得编码错误.

`paramstyle`

String constant stating the type of parameter marker formatting expected by the interface. Set to 'format' = ANSI C printf format codes, e.g. '...WHERE name=%s'. If a mapping object is used for conn.execute(), then the interface actually uses 'pyformat' = Python extended format codes, e.g. '...WHERE name=%(name)s'. However, the API does not presently allow the specification of more than one style in paramstyle.

字符串静态变量声明了接口希望的 参数格式化格式. 设置 `format` 代表 ANSI C `printf` 格式化编码, 比如：`...WHERE name=%s`. 如果`conn.execute()`使用一个 映射对象, 然后接口实际上使用`pyformat`, 代表 用Python扩展格式化编码，比如`...WHERE name=%(name)s'. 但是,  目前 API不允许`paramstyle`指定多个风格.

Note that any literal percent signs in the query string passed to execute() must be escaped, i.e. %%.

注意, 在查询语句中, 任何的传入`execute()`字符百分比标示都需要转义, 比如 `%%`

Parameter placeholders can only be used to insert column values. They can not be used for other parts of SQL, such as table names, statements, etc.

参数占位符 只能用于插入 *column*列值. 不能用于SQL的其他, 比如 表名, 声明, 等.

> 译者，这个参数是很有用的，我之前的工作使用了一个web.py二次封装的框架, 其中直接写原生SQL, 直接只用MySQLdb是我推荐喜欢的风格. 
> 当时在使用MySQLdb的时候, 如果能用好这些参数，如果能认真理解文档，我想能走得更远.
 

`conv`

A dictionary or mapping which controls how types are converted from MySQL to Python and vice versa.

一个字典 或 映射类型 定义了 如何从MySQL 到 Python之间的类型转换，反之亦然.

If the key is a MySQL type (from FIELD_TYPE.*), then the value can be either:

如果 键是一个MySQL类型(从 `FIELD_TYPE.*`), 那么值可以是:

a callable object which takes a string argument (the MySQL value),' returning a Python value

一个接受 *字符串参数(MySQL 值)*的可调用对象，返回一个Python值

a sequence of 2-tuples, where the first value is a combination of flags from MySQLdb.constants.FLAG, and the second value is a function as above. The sequence is tested until the flags on the field match those of the first value. If both values are None, then the default conversion is done. Presently this is only used to distinquish TEXT and BLOB columns.

一个 含有2个tuple的列表, 第一个值是一个从`MySQLdb.constants.FLAG`得到的组合, 第二个值是一个同上的 function函数. 遍历这个列表，直到在字段与第一个值相匹配. 如果两个值都为None, 那么执行默认的转换. 目前这只用于区分 `TEXT` 和 `BLOB`字段.

If the key is a Python type or class, then the value is a callable Python object (usually a function) taking two arguments (value to convert, and the conversion dictionary) which converts values of this type to a SQL literal string value.

如果键是一个Python类型 或 类, 那么值是一个 可调用的Python对象(通常是一个function函数) 接受两个参数(1. 用来转化的值, 2. 用来转换的字典)，她转换 某一类型的值 为一个SQL的字符串值.

This is initialized with reasonable defaults for most types. When creating a Connection object, you can pass your own type converter dictionary as a keyword parameter. Otherwise, it uses a copy of MySQLdb.converters.conversions. Several non-standard types are returned as strings, which is how MySQL returns all columns. For more details, see the built-in module documentation.

这个值初始化为一个对大部分类型都合理的默认值. 当建立一个连接对象, 你可以传一个你自己的 *类型转化字典* 作为 *关键字参数*. 否则, 默认用一个`MySQLdb.converters.conversions`的拷贝. 几个非标准的类型作为字符串返回, 她就是MySQL怎样返回所有列的. 为了更多地细节, 查看 内置库 文档.


### Connection Objects

Connection objects are returned by the connect() function.

`Connection`对象通过`connect()`函数 返回.

`commit()`

If the database and the tables support transactions, this commits the current transaction; otherwise this method successfully does nothing.

如果数据库 和 表支持事务, 会提交当前的事务; 否则method直接执行成功, 什么都不做.

`rollback()`

If the database and tables support transactions, this rolls back (cancels) the current transaction; otherwise a NotSupportedError is raised.

cursor([cursorclass])
MySQL does not support cursors; however, cursors are easily emulated. You can supply an alternative cursor class as an optional parameter. If this is not present, it defaults to the value given when creating the connection object, or the standard Cursor class. Also see the additional supplied cursor classes in the usage section.
There are many more methods defined on the connection object which are MySQL-specific. For more information on them, consult the internal documentation using pydoc.

###  Cursor Objects

callproc(procname, args)
Calls stored procedure procname with the sequence of arguments in args. Returns the original arguments. Stored procedure support only works with MySQL-5.0 and newer.

Compatibility note: PEP-249 specifies that if there are OUT or INOUT parameters, the modified values are to be returned. This is not consistently possible with MySQL. Stored procedure arguments must be passed as server variables, and can only be returned with a SELECT statement. Since a stored procedure may return zero or more result sets, it is impossible for MySQLdb to determine if there are result sets to fetch before the modified parmeters are accessible.

The parameters are stored in the server as @_*procname*_*n*, where n is the position of the parameter. I.e., if you cursor.callproc('foo', (a, b, c)), the parameters will be accessible by a SELECT statement as @_foo_0, @_foo_1, and @_foo_2.

Compatibility note: It appears that the mere act of executing the CALL statement produces an empty result set, which appears after any result sets which might be generated by the stored procedure. Thus, you will always need to use nextset() to advance result sets.

close()
Closes the cursor. Future operations raise ProgrammingError. If you are using server-side cursors, it is very important to close the cursor when you are done with it and before creating a new one.
info()
Returns some information about the last query. Normally you don't need to check this. If there are any MySQL warnings, it will cause a Warning to be issued through the Python warning module. By default, Warning causes a message to appear on the console. However, it is possible to filter these out or cause Warning to be raised as exception. See the MySQL docs for mysql_info(), and the Python warning module. (Non-standard)
setinputsizes()
Does nothing, successfully.
setoutputsizes()
Does nothing, successfully.
nextset()
Advances the cursor to the next result set, discarding the remaining rows in the current result set. If there are no additional result sets, it returns None; otherwise it returns a true value.

Note that MySQL doesn't support multiple result sets until 4.1.

### Some examples

The connect() method works nearly the same as with _mysql:

import MySQLdb
db=MySQLdb.connect(passwd="moonpie",db="thangs")
To perform a query, you first need a cursor, and then you can execute queries on it:

c=db.cursor()
max_price=5
c.execute("""SELECT spam, eggs, sausage FROM breakfast
          WHERE price < %s""", (max_price,))
In this example, max_price=5 Why, then, use %s in the string? Because MySQLdb will convert it to a SQL literal value, which is the string '5'. When it's finished, the query will actually say, "...WHERE price < 5".

Why the tuple? Because the DB API requires you to pass in any parameters as a sequence. Due to the design of the parser, (max_price) is interpreted as using algebraic grouping and simply as max_price and not a tuple. Adding a comma, i.e. (max_price,) forces it to make a tuple.

And now, the results:

>>> c.fetchone()
(3L, 2L, 0L)
Quite unlike the _mysql example, this returns a single tuple, which is the row, and the values are properly converted by default... except... What's with the L's?

As mentioned earlier, while MySQL's INTEGER column translates perfectly into a Python integer, UNSIGNED INTEGER could overflow, so these values are converted to Python long integers instead.

If you wanted more rows, you could use c.fetchmany(n) or c.fetchall(). These do exactly what you think they do. On c.fetchmany(n), the n is optional and defaults toc.arraysize, which is normally 1. Both of these methods return a sequence of rows, or an empty sequence if there are no more rows. If you use a weird cursor class, the rows themselves might not be tuples.

Note that in contrast to the above, c.fetchone() returns None when there are no more rows to fetch.

The only other method you are very likely to use is when you have to do a multi-row insert:

c.executemany(
      """INSERT INTO breakfast (name, spam, eggs, sausage, price)
      VALUES (%s, %s, %s, %s, %s)""",
      [
      ("Spam and Sausage Lover's Plate", 5, 1, 8, 7.95 ),
      ("Not So Much Spam Plate", 3, 2, 0, 3.95 ),
      ("Don't Wany ANY SPAM! Plate", 0, 4, 3, 5.95 )
      ] )
Here we are inserting three rows of five values. Notice that there is a mix of types (strings, ints, floats) though we still only use %s. And also note that we only included format strings for one row. MySQLdb picks those out and duplicates them for each row.

### Using and extending

In general, it is probably wise to not directly interact with the DB API except for small applicatons. Databases, even SQL databases, vary widely in capabilities and may have non-standard features. The DB API does a good job of providing a reasonably portable interface but some methods are non-portable. Specifically, the parameters accepted by connect() are completely implementation-dependent.

If you believe your application may need to run on several different databases, the author recommends the following approach, based on personal experience: Write a simplified API for your application which implements the specific queries and operations your application needs to perform. Implement this API as a base class which should be have few database dependencies, and then derive a subclass from this which implements the necessary dependencies. In this way, porting your application to a new database should be a relatively simple matter of creating a new subclass, assuming the new database is reasonably standard.

Because MySQLdb's Connection and Cursor objects are written in Python, you can easily derive your own subclasses. There are several Cursor classes in MySQLdb.cursors:

BaseCursor
The base class for Cursor objects. This does not raise Warnings.
CursorStoreResultMixIn
Causes the Cursor to use the mysql_store_result() function to get the query result. The entire result set is stored on the client side.
CursorUseResultMixIn
Causes the cursor to use the mysql_use_result() function to get the query result. The result set is stored on the server side and is transferred row by row using fetch operations.
CursorTupleRowsMixIn
Causes the cursor to return rows as a tuple of the column values.
CursorDictRowsMixIn

Causes the cursor to return rows as a dictionary, where the keys are column names and the values are column values. Note that if the column names are not unique, i.e., you are selecting from two tables that share column names, some of them will be rewritten as table.column. This can be avoided by using the SQL AS keyword. (This is yet-another reason not to use * in SQL queries, particularly where JOIN is involved.)
Cursor
The default cursor class. This class is composed of CursorWarningMixIn, CursorStoreResultMixIn, CursorTupleRowsMixIn, and BaseCursor, i.e. it raises Warning, usesmysql_store_result(), and returns rows as tuples.
DictCursor
Like Cursor except it returns rows as dictionaries.
SSCursor
A "server-side" cursor. Like Cursor but uses CursorUseResultMixIn. Use only if you are dealing with potentially large result sets.
SSDictCursor
Like SSCursor except it returns rows as dictionaries.


### Embedded Server

Instead of connecting to a stand-alone server over the network, the embedded server support lets you run a full server right in your Python code or application server.

If you have built MySQLdb with embedded server support, there are two additional functions you will need to make use of:

server_init(args, groups)
Initialize embedded server. If this client is not linked against the embedded server library, this function does nothing.

args
sequence of command-line arguments
groups
sequence of groups to use in defaults files
server_end()
Shut down embedded server. If not using an embedded server, this does nothing.
See the MySQL documentation for more information on the embedded server.

Title:	MySQLdb: a Python interface for MySQL
Author:	Andy Dustman
Version:	$Revision: 421 $
