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
> conn methods 看下表

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

db=_mysql.connect(host="localhost",user="joebob",
                  passwd="moonpie",db="thangs")