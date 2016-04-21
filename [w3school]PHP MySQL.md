#PHP MySQL
@(PHP)[马克飞象, PHP, MySQL]
老生常谈：**MySQL**是最流行的开源数据库服务器
熟能生巧：对数据库的操作（**增** **删** **改** **查** ）
<hr>
###什么是**MySQL**？
**MySQL **是一种数据库。数据库定义了存储信息的结构。
在**数据库**中，存在着一些**表**。**类似 HTML 表格**，**数据库**表含有**行**、**列**以及**单元**。
在**分类存储**信息时，**数据库**非常有用。一个公司的数据库可能拥有这些表："Employees", "Products", "Customers" 以及 "Orders"。
###数据库表
**数据库**通常包含*一个*或*多个* **表**。每个**表都一个名称**（比如 "Customers" 或 "Orders"）。每个**表包含带有数据的记录（行）**。

LastName | FirstName |    Address   |  City
---------|-----------|--------------|---------
Hansen   | Ola       | Timoteivn 10 | Sandnes
Svendson | Tove      | Borgvn 23    | Sandnes
Pettersen| Kari      | Storgt 20    | Stavanger

上面的**表**含有三个记录（每个记录是一个人）和四个列（LastName, FirstName, Address 以及 City）。
###查询
查询是**一种询问或请求**。
通过 **MySQL**，我们可以**向数据库查询具体的信息**，并**得到返回的记录集**。
一条查询语句：
`SELECT LastName FROM Persons`
上面的查询选取了 **Persons** 表中 **LastName** 列的所有数据，并返回类似这样的记录集：

|LastName
|---
|Hansen
|Svendson
|Pettersen

###PHP 连接到一个 MySQL 数据库
在您能够访问并处理数据库中的数据之前，您必须创建到达**数据库的连接**。
在 PHP 中，这个任务通过 `mysql_connect()` 函数完成。
语法：
`mysql_connect(servername,username,password);`
参数   |  描述
-|
servername | 可选。规定要连接的服务器。默认是 "localhost:3306"。
username  |  可选。规定登录所使用的用户名。默认值是拥有服务器进程的用户的名称。
password  |  可选。规定登录所用的密码。默认是 ""。
注释：虽然还存在其他的参数，但上面列出了最重要的参数。请访问 [W3School 提供的 PHP MySQL 参考手册](http://www.w3school.com.cn/php/php_ref_mysql.asp)，获得更多的细节信息。
例子
在下面的例子中，我们在一个变量中 (`$con`) **存放了在脚本中供稍后使用的连接**。如果连接失败，将执行 "die" (`stop()`函数的别名)部分：
```php
<?php
$con = mysql_connect("localhost","peter","abc123");
if (!$con)
  {
  die('Could not connect: ' . mysql_error());
  }

// some code

?>
```
###关闭连接
脚本一结束，就会关闭连接。**如需提前关闭连接**，请使用 `mysql_close()` 函数。
```php
<?php
$con = mysql_connect("localhost","peter","abc123");
if (!$con)
  {
  die('Could not connect: ' . mysql_error());
  }

// some code

mysql_close($con);
?>
```
###创建数据库
`CREATE DATABASE` 语句用于在 MySQL 中创建数据库。
语法：
`CREATE DATABASE database_name`
为了让 **PHP** 执行上面的语句，我们必须使用 `mysql_query()` 函数。**此函数用于向 MySQL 连接发送查询或命令**。
例子：
（创建了一个名为 "my_db" 的数据库）
```php
<?php
$con = mysql_connect("localhost","peter","abc123");
if (!$con)
  {
  die('Could not connect: ' . mysql_error());
  }

if (mysql_query("CREATE DATABASE my_db",$con))
  {
  echo "Database created";
  }
else
  {
  echo "Error creating database: " . mysql_error();
  }

mysql_close($con);
?>
```
###创建表
`CREATE TABLE` 用于在 **MySQL** 中创建数据库表。
语法：
```sql
CREATE TABLE table_name
(
column_name1 data_type,
column_name2 data_type,
column_name3 data_type,
.......
)
```
为了执行此命令，我必须向 `mysql_query()` 函数添加 `CREATE TABLE` 语句。

例子：
（创建一个名为 "Persons" 的表，此表有三列。列名是 "FirstName", "LastName" 以及 "Age"）
```php
<?php
$con = mysql_connect("localhost","peter","abc123");
if (!$con)
  {
  die('Could not connect: ' . mysql_error());
  }

// Create database
if (mysql_query("CREATE DATABASE my_db",$con))
  {
  echo "Database created";
  }
else
  {
  echo "Error creating database: " . mysql_error();
  }

// Create table in my_db database
mysql_select_db("my_db", $con);
$sql = "CREATE TABLE Persons 
(
FirstName varchar(15),
LastName varchar(15),
Age int
)";
mysql_query($sql,$con);

mysql_close($con);
?>
```
<b style="color:red">重要事项：</b>在创建表之前，必须首先选择数据库。通过 `mysql_select_db()` 函数选取数据库。

###MySQL 数据类型
数值类型 | 描述
-|
int(size)<br>smallint(size)<br>tinyint(size)<br>mediumint(size)<br>bigint(size)			|	仅支持整数。在 size 参数中规定数字的最大值。
decimal(size,d)<br>double(size,d)<br>float(size,d)			|支持带有小数的数字。在 size 参数中规定数字的最大值。在 d 参数中规定小数点右侧的数字的最大值。

文本数据类型 | 描述
-|
char(size) | 支持固定长度的字符串。（可包含字母、数字以及特殊符号）。在 size 参数中规定固定长度。
 varchar(size) | 支持可变长度的字符串。（可包含字母、数字以及特殊符号）。在 size 参数中规定最大长度。
 tinytext | 支持可变长度的字符串，最大长度是 255 个字符。
 text<br>blob | 支持可变长度的字符串，最大长度是 65535 个字符。
 mediumtext<br>mediumblob | 支持可变长度的字符串，最大长度是 16777215 个字符。
 longtext<br>longblob | 支持可变长度的字符串，最大长度是 4294967295 个字符。

日期数据类型 | 描述
-|
date(yyyy-mm-dd)<br>datetime(yyyy-mm-dd hh:mm:ss)<br>timestamp(yyyymmddhhmmss)<br>time(hh:mm:ss) | 支持日期或时间

杂项数据类型 | 描述
-|
enum(value1,value2,ect) | ENUM 是 ENUMERATED 列表的缩写。可以在括号中存放最多 65535 个值。
set | SET 与 ENUM 相似。但是，SET 可拥有最多 64 个列表项目，并可存放不止一个 choice


###主键和自动递增字段
每个表都应有一个**主键字段**。
**主键**用于对表中的**行**进行**唯一标识**。每个**主键值在表中必须是唯一**的。此外，**主键字段不能为空**，这是由于数据库引擎需要一个值来对记录进行定位。
**主键字段永远要被编入索引**。这条规则没有例外。你必须对主键字段进行索引，这样数据库引擎才能快速定位给予该键值的行。（比如我们常用的 phpcms 用户主键userid 和 栏目主键 catid）
下面的例子把 personID 字段设置为主键字段。**主键字段通常是 ID 号**，且通常使用 `AUTO_INCREMENT` 设置。
`AUTO_INCREMENT` 会在新记录被添加时逐一增加该字段的值（自增不可逆）要确保主键字段不为空，我们**必须向该字段添加 NOT NULL 设置**。
例子：
```sql
$sql = "CREATE TABLE Persons 
(
personID int NOT NULL AUTO_INCREMENT, 
PRIMARY KEY(personID),
FirstName varchar(15),
LastName varchar(15),
Age int
)";

mysql_query($sql,$con);
```
###向数据库表插入数据
`INSERT INTO` 语句用于向数据库表添加新记录。
语法：
```sql
INSERT INTO table_name
VALUES (value1, value2,....)
```
还可以规定希望在其中插入数据的列：
```sql
INSERT INTO table_name (column1, column2,...)
VALUES (value1, value2,....)
```
**注释**：SQL 语句**对大小写不敏感**。`INSERT INTO` 与 `insert into` 相同。
为了让 **PHP** 执行该语句，我们必须使用 `mysql_query()` 函数。
例子：
（创建了一个名为 "Persons" 的表，有三个列："Firstname", "Lastname" 以及 "Age"。我们将在本例中使用同样的表。下面的例子向 "Persons" 表添加了两个新记录）
```php
<?php
$con = mysql_connect("localhost","peter","abc123");
if (!$con)
  {
  die('Could not connect: ' . mysql_error());
  }

mysql_select_db("my_db", $con);

mysql_query("INSERT INTO Persons (FirstName, LastName, Age) 
VALUES ('Peter', 'Griffin', '35')");

mysql_query("INSERT INTO Persons (FirstName, LastName, Age) 
VALUES ('Glenn', 'Quagmire', '33')");

mysql_close($con);
?>
```
###把来自表单的数据插入数据库
创建一个 **HTML 表单**，这个表单可把新记录插入 "Persons" 表。
 HTML 表单：
```javascript
<html>
<body>

<form action="insert.php" method="post">
Firstname: <input type="text" name="firstname" />
Lastname: <input type="text" name="lastname" />
Age: <input type="text" name="age" />
<input type="submit" />
</form>

</body>
</html>
```
当用户**点击上例中 HTML 表单中的提交按钮时**，表单数据被发送到 **"insert.php"**。**"insert.php"** 文件连接数据库，并通过 `$_POST `变量从表单取回值。然后，`mysql_query()` 函数执行 `INSERT INTO` 语句，**一条新的记录会添加到数据库表中**。
下面是 **"insert.php"** 页面的代码：
```php
<?php
$con = mysql_connect("localhost","peter","abc123");
if (!$con)
  {
  die('Could not connect: ' . mysql_error());
  }

mysql_select_db("my_db", $con);

$sql="INSERT INTO Persons (FirstName, LastName, Age)
VALUES
('$_POST[firstname]','$_POST[lastname]','$_POST[age]')";

if (!mysql_query($sql,$con))
  {
  die('Error: ' . mysql_error());
  }
echo "1 record added";

mysql_close($con)
?>
```
###从数据库表中选取数据
`SELECT` 语句用于从数据库中选取数据。
语法：
`SELECT column_name(s) FROM table_name`
**注释**：SQL 语句对大小写不敏感。`SELECT`与` select `等效。
为了让 **PHP** 执行上面的语句，我们必须使用 `mysql_query()` 函数。
例子：
（选取存储在 "Persons" 表中的所有数据（`*` **字符选取表中所有数据**））
```php
<?php
$con = mysql_connect("localhost","peter","abc123");
if (!$con)
  {
  die('Could not connect: ' . mysql_error());
  }

mysql_select_db("my_db", $con);

$result = mysql_query("SELECT * FROM Persons");

while($row = mysql_fetch_array($result))
  {
  echo $row['FirstName'] . " " . $row['LastName'];
  echo "<br />";
  }

mysql_close($con);
?>
```
上面这个例子在 `$result` 变量中存放由 `mysql_query()` 函数返回的数据。接下来，我们使用 `mysql_fetch_array()` 函数以数组的形式从记录集返回第一行。每个随后对 `mysql_fetch_array()` 函数的调用都会返回记录集中的下一行。 `while` loop 语句会循环记录集中的所有记录。为了输出每行的值，我们使用了 PHP 的 `$row 变量` (`$row['FirstName']` 和 `$row['LastName']`)。
###在 HTML 表格中显示结果
下面的例子选取的数据与上面的例子相同，但是将把数据显示在一个 HTML 表格中：
```php
$con = mysql_connect("localhost","peter","abc123");
if (!$con)
  {
  die('Could not connect: ' . mysql_error());
  }

mysql_select_db("my_db", $con);

$result = mysql_query("SELECT * FROM Persons");

echo "<table border='1'>
<tr>
<th>Firstname</th>
<th>Lastname</th>
</tr>";

while($row = mysql_fetch_array($result))
  {
  echo "<tr>";
  echo "<td>" . $row['FirstName'] . "</td>";
  echo "<td>" . $row['LastName'] . "</td>";
  echo "</tr>";
  }
echo "</table>";

mysql_close($con);
```
###WHERE 子句(条件)
如需选取匹配指定条件的数据，请向 **SELECT** 语句添加 **WHERE** 子句。
```sql
SELECT column FROM table
WHERE column operator value
```
下面的运算符可与`WHERE` 子句一起使用：
运算符 | 说明
-|
= | 等于
!= | 不等于
> | 大于
<  | 小于
>= | 大于或等于
<= | 小于或等于
BETWEEN | 介于一个包含范围内
LIKE | 搜索匹配的模式
**注释**：SQL 语句对大小写不敏感。`WHERE` 与 `where` 等效。
**PHP**使用 `mysql_query()` 函数
例子：
（从 "Persons" 表中选取所有 FirstName='Peter' 的行）
```php
<?php
$con = mysql_connect("localhost","peter","abc123");
if (!$con)
  {
  die('Could not connect: ' . mysql_error());
  }

mysql_select_db("my_db", $con);

$result = mysql_query("SELECT * FROM Persons
WHERE FirstName='Peter'");

while($row = mysql_fetch_array($result))
  {
  echo $row['FirstName'] . " " . $row['LastName'];
  echo "<br />";
  }

?>
```
###ORDER BY 关键词
`ORDER BY` 关键词用于对记录集中的数据进行**排序**。
语法：
```sql
SELECT column_name(s)
FROM table_name
ORDER BY column_name
```
注释：SQL 对大小写不敏感。
例子：
（ "Persons" 表中的存储的所有数据，并根据 "Age" 列对结果进行排序）
```php
<?php
$con = mysql_connect("localhost","peter","abc123");
if (!$con)
  {
  die('Could not connect: ' . mysql_error());
  }

mysql_select_db("my_db", $con);

$result = mysql_query("SELECT * FROM Persons ORDER BY age");

while($row = mysql_fetch_array($result))
  {
  echo $row['FirstName'];
  echo " " . $row['LastName'];
  echo " " . $row['Age'];
  echo "<br />";
  }

mysql_close($con);
?>
```
###升序或降序的排序
如果您使用 `ORDER BY` 关键词，记录集的排序顺序默认是升序（1 在 9 之前，"a" 在 "p" 之前）大概指由低到高。
请使用 `DESC` 关键词来设定降序排序（9 在 1 之前，"p" 在 "a" 之前）大概指由高到低。
```sql
SELECT column_name(s)
FROM table_name
ORDER BY column_name DESC
```
###根据两列进行排序
可以根据多个列进行排序。**当按照多个列进行排序时，只有第一列相同时才使用第二列**：
```sql
SELECT column_name(s)
FROM table_name
ORDER BY column_name1, column_name2
```
###更新数据库中的数据
`UPDATE` 语句用于在数据库表中修改数据。
语法：
```sql
UPDATE table_name
SET column_name = new_value
WHERE column_name = some_value
```
**注释**：SQL 对大小写不敏感。
PHP 必须使用 `mysql_query()`函数
例子：
```php
<?php
$con = mysql_connect("localhost","peter","abc123");
if (!$con)
  {
  die('Could not connect: ' . mysql_error());
  }

mysql_select_db("my_db", $con);

mysql_query("UPDATE Persons SET Age = '36'
WHERE FirstName = 'Peter' AND LastName = 'Griffin'");

mysql_close($con);
?>
```
将字段 FirstName = 'Peter'  并且 LastName = 'Griffin' 的人的 Age 字段 修改为 “36”
<hr>
###删除数据库中的数据
`DELETE FROM` 语句用于从数据库表中删除记录。
语法：
```sql
DELETE FROM table_name
WHERE column_name = some_value
```
注意点和上面的一样敏感和PHP的某个函数
例子：
```php
<?php
$con = mysql_connect("localhost","peter","abc123");
if (!$con)
  {
  die('Could not connect: ' . mysql_error());
  }

mysql_select_db("my_db", $con);

mysql_query("DELETE FROM Persons WHERE LastName='Griffin'");

mysql_close($con);
?>
```
删除了条件满足字段 LastName='Griffin' 的数据行
<hr>
###PHP Database DOBC
**ODBC 是一种应用程序编程接口（Application Programming Interface，API），使我们有能力连接到某个数据源（比如一个 MS Access 数据库）。 **
###创建 ODBC 连接
通过一个 **ODBC** 连接，您可以连接到您的网络中的任何计算机上的任何数据库，只要 **ODBC** 连接是可用的。
这是创建到达 MS Access 数据的 ODBC 连接的方法：
1. 在控制面板中打开管理工具
2. 双击其中的数据源 (ODBC) 图标
3. 选择系统 DSN 选项卡
4. 点击系统 DSN 选项卡中的“添加”按钮
5. 选择 Microsoft Access Driver。点击完成。
6. 在下一个界面，点击“选择”来定位数据库。
7. 为这个数据库取一个数据源名 (DSN)。
8. 点击确定。

请注意，必须在您的网站所在的计算机上完成这个配置。如果您的计算机上正在运行 Internet 信息服务器 (IIS)，上面的指令会生效，但是假如您的网站位于远程服务器，您必须拥有对该服务器的物理访问权限，或者请您的主机提供商为您建立 DSN。

###连接到 ODBC
`odbc_connect()` 函数用于连接到 `ODBC` 数据源。该函数有四个参数：**数据源名**、**用户名**、**密码**以及**可选的指针类型**参数。
`odbc_exec()` 函数用于执行 SQL 语句。
例子：
下面的例子创建了到达名为 **northwind** 的 **DSN** 的连接，没有用户名和密码。然后创建并执行一条 SQL 语句：
```php
$conn=odbc_connect('northwind','','');
$sql="SELECT * FROM customers"; 
$rs=odbc_exec($conn,$sql);
```
###取回记录
`odbc_fetch_row()` 函数用于从结果集中返回记录。如果能够返回行，则返回 `true`，否则返回 `false`。
该函数有两个参数：ODBC 结果标识符和可选的行号：
`odbc_fetch_row($rs)`
###从记录中取回字段
`odbc_result()` 函数用于从记录中读取字段。该函数有两个参数：ODBC 结果标识符和字段编号或名称。
下面的代码行从记录中返回第一个字段的值：
`$compname=odbc_result($rs,1); `
The code line below returns the value of a field called "CompanyName":
`$compname=odbc_result($rs,"CompanyName");`
###关闭 ODBC 连接
`odbc_close()`函数用于关闭 ODBC 连接。
`odbc_close($conn);`
###ODBC 实例
下面的例子展示了如何首先创建一个数据库连接，然后是结果集，然后在 HTML 表格中显示数据
```javascript
<html>
<body>

<?php
$conn=odbc_connect('northwind','','');
if (!$conn)
  {exit("Connection Failed: " . $conn);}
$sql="SELECT * FROM customers";
$rs=odbc_exec($conn,$sql);
if (!$rs)
  {exit("Error in SQL");}
echo "<table><tr>";
echo "<th>Companyname</th>";
echo "<th>Contactname</th></tr>";
while (odbc_fetch_row($rs))
{
  $compname=odbc_result($rs,"CompanyName");
  $conname=odbc_result($rs,"ContactName");
  echo "<tr><td>$compname</td>";
  echo "<td>$conname</td></tr>";
}
odbc_close($conn);
echo "</table>";
?>

</body>
</html>
```

