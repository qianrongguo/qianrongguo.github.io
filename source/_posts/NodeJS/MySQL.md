
## 安装MySQL驱动程序
`npm install mysql`


## 建立连接

```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword"
});

con.connect(function(err) {
  if (err) throw err;
  console.log("Connected!");
});
```


## 查询数据库

```js
con.connect(function(err) {
  if (err) throw err;
  console.log("Connected!");
  con.query(sql, function (err, result) {
    if (err) throw err;
    console.log("Result: " + result);
  });
});
```

## 创建数据库

要在MySQL中创建数据库，请使用`CREATE DATABASE`语句：

`创建一个名为“ mydb”的数据库：`
```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword"
});

con.connect(function(err) {
  if (err) throw err;
  console.log("Connected!");
  con.query("CREATE DATABASE mydb", function (err, result) {
    if (err) throw err;
    console.log("Database created");
  });
});
```
## 创建表格

要在MySQL中创建表，请使用`CREATE TABLE`语句。
确保在创建连接时定义数据库的名称。

`创建一个名为“ customers”的表：`
```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  console.log("Connected!");
  var sql = "CREATE TABLE customers (name VARCHAR(255), address VARCHAR(255))";
  con.query(sql, function (err, result) {
    if (err) throw err;
    console.log("Table created");
  });
});
```

## primary key
创建表时，还应为每个记录创建一个具有唯一键的列。

这可以通过将列定义为`INT AUTO_INCREMENT PRIMARY KEY`来完成，该列将为每个记录插入一个唯一编号。从1开始，每条记录增加1。

`创建表时创建主键：`
```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  console.log("Connected!");
  var sql = "CREATE TABLE customers (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(255), address VARCHAR(255))";
  con.query(sql, function (err, result) {
    if (err) throw err;
    console.log("Table created");
  });
});
```

如果该表已经存在，请使用ALTER TABLE关键字：

`在现有表上创建主键：`
```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  console.log("Connected!");
  var sql = "ALTER TABLE customers ADD COLUMN id INT AUTO_INCREMENT PRIMARY KEY";
  con.query(sql, function (err, result) {
    if (err) throw err;
    console.log("Table altered");
  });
});
```

## 插入表格

`在“customers”表中插入一条记录：`

```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  console.log("Connected!");
  var sql = "INSERT INTO customers (name, address) VALUES ('Company Inc', 'Highway 37')";
  con.query(sql, function (err, result) {
    if (err) throw err;
    console.log("1 record inserted");
  });
});
```


## 插入多条记录

要插入多个记录，请创建一个包含值的数组，然后在sql中插入问号，该问号将被值数组替换：
`INSERT INTO customers (name, address) VALUES ?`

`用数据填充“客户”表：`

```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  console.log("Connected!");
  var sql = "INSERT INTO customers (name, address) VALUES ?";
  var values = [
    ['John', 'Highway 71'],
    ['Peter', 'Lowstreet 4'],
    ['Amy', 'Apple st 652'],
    ['Hannah', 'Mountain 21'],
    ['Michael', 'Valley 345'],
    ['Sandy', 'Ocean blvd 2'],
    ['Betty', 'Green Grass 1'],
    ['Richard', 'Sky st 331'],
    ['Susan', 'One way 98'],
    ['Vicky', 'Yellow Garden 2'],
    ['Ben', 'Park Lane 38'],
    ['William', 'Central st 954'],
    ['Chuck', 'Main Road 989'],
    ['Viola', 'Sideway 1633']
  ];
  con.query(sql, [values], function (err, result) {
    if (err) throw err;
    console.log("Number of records inserted: " + result.affectedRows);
  });
});
```

## 结果对象

执行查询时，将返回结果对象。

结果对象包含有关查询如何影响表的信息。

从上面的示例返回的结果对象如下所示：

```json
{
  fieldCount: 0,
  affectedRows: 14,
  insertId: 0,
  serverStatus: 2,
  warningCount: 0,
  message: '\'Records:14  Duplicated: 0  Warnings: 0',
  protocol41: true,
  changedRows: 0
}
```
属性值可以这样显示

`返回受影响的行数`:
```js
console.log(result.affectedRows)
```

## 获取插入的ID

对于具有自动递增ID字段的表，可以通过询问结果对象来获取刚插入的行的ID。

**注意：为了能够获得插入的ID，只能插入一行。**

```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  var sql = "INSERT INTO customers (name, address) VALUES ('Michelle', 'Blue Village 1')";
  con.query(sql, function (err, result) {
    if (err) throw err;
    console.log("1 record inserted, ID: " + result.insertId);
  });
});
```


# 查询表（select from table）

1.要从MySQL中的表中选择数据，请使用“ SELECT”语句。

`从“customers”表中选择所有记录，并显示结果对象：`
```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  con.query("SELECT * FROM customers", function (err, result, fields) {
    if (err) throw err;
    console.log(result);
  });
});
```

**SELECT 将返回所有列**

2.要仅选择表中的某些列，请使用“ SELECT”语句，后跟列名

`从“customers”表中选择名称和地址，并显示返回对象：`
```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  con.query("SELECT name, address FROM customers", function (err, result, fields) {
    if (err) throw err;
    console.log(result);
  });
});

```

结果
```js
[
  { name: 'John', address: 'Highway 71'},
  { name: 'Peter', address: 'Lowstreet 4'},
  { name: 'Amy', address: 'Apple st 652'},
  { name: 'Hannah', address: 'Mountain 21'},
  { name: 'Michael', address: 'Valley 345'},
  { name: 'Sandy', address: 'Ocean blvd 2'},
  { name: 'Betty', address: 'Green Grass 1'},
  { name: 'Richard', address: 'Sky st 331'},
  { name: 'Susan', address: 'One way 98'},
  { name: 'Vicky', address: 'Yellow Garden 2'},
  { name: 'Ben', address: 'Park Lane 38'},
  { name: 'William', address: 'Central st 954'},
  { name: 'Chuck', address: 'Main Road 989'},
  { name: 'Viola', address: 'Sideway 1633'}
]
```

## 结果对象

从上面示例的结果中可以看到，结果对象是一个包含每一行作为对象的数组。

要返回例如第三条记录的地址，只需参考第三条数组对象的address属性：

`返回第三条记录的地址：`
```js
console.log(result[2].address);
```

## 字段对象

回调函数的第三个参数是一个数组，其中包含有关结果中每个字段的信息。

`从“customers”表中选择所有记录，然后显示字段对象：`

```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  con.query("SELECT name, address FROM customers", function (err, result, fields) {
    if (err) throw err;
    console.log(fields);
  });
});
```
结果
```js
[
  {
    catalog: 'def',
    db: 'mydb',
    table: 'customers',
    orgTable: 'customers',
    name: 'name',
    orgName: 'address',
    charsetNr: 33,
    length: 765,
    type: 253,
    flags: 0,
    decimals: 0,
    default: undefined,
    zeroFill: false,
    protocol41: true
  },
  {
    catalog: 'def',
    db: 'mydb',
    table: 'customers',
    orgTable: 'customers',
    name: 'address',
    orgName: 'address',
    charsetNr: 33,
    length: 765,
    type: 253,
    flags: 0,
    decimals: 0,
    default: undefined,
    zeroFill: false,
    protocol41: true
  {
]
```

# where

## 选择带过滤器
从表中选择记录时，可以使用“ WHERE”语句过滤选择：

`选择地址为“ Park Lane 38”的记录：`
```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  con.query("SELECT * FROM customers WHERE address = 'Park Lane 38'", function (err, result) {
    if (err) throw err;
    console.log(result);
  });
});
```
## 通配符
您也可以选择以给定字母或短语开头，包含或结尾的记录。

使用'％'通配符表示零个，一个或多个字符：

`选择地址以字母“ S”开头的记录：`
```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  con.query("SELECT * FROM customers WHERE address LIKE 'S%'", function (err, result) {
    if (err) throw err;
    console.log(result);
  });
});
```

## 转义查询值
当查询值是用户提供的变量时，应转义这些值。

这是为了防止SQL注入，这是破坏或滥用数据库的常见Web黑客技术。

MySQL模块具有以下方法来转义查询值：

`使用以下mysql.escape() 方法转义查询值：`
```js
var adr = 'Mountain 21';
var sql = 'SELECT * FROM customers WHERE address = ' + mysql.escape(adr);
con.query(sql, function (err, result) {
  if (err) throw err;
  console.log(result);
});
```

`通过使用占位符? 方法转义查询值：`
```js
var adr = 'Mountain 21';
var sql = 'SELECT * FROM customers WHERE address = ?';
con.query(sql, [adr], function (err, result) {
  if (err) throw err;
  console.log(result);
});
```
如果您有多个占位符，则数组按该顺序包含多个值：
```js
var name = 'Amy';
var adr = 'Mountain 21';
var sql = 'SELECT * FROM customers WHERE name = ? OR address = ?';
con.query(sql, [name, adr], function (err, result) {
  if (err) throw err;
  console.log(result);
});
```

# sort result

使用ORDER BY语句对结果进行升序或降序排序。

缺省情况下，ORDER BY关键字对结果进行升序排序。要按降序对结果进行排序，请使用DESC关键字。

`按名称的字母顺序对结果进行排序：`
```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  con.query("SELECT * FROM customers ORDER BY name", function (err, result) {
    if (err) throw err;
    console.log(result);
  });
});
```
结果
```js
[
  { id: 3, name: 'Amy', address: 'Apple st 652'},
  { id: 11, name: 'Ben', address: 'Park Lane 38'},
  { id: 7, name: 'Betty', address: 'Green Grass 1'},
  { id: 13, name: 'Chuck', address: 'Main Road 989'},
  { id: 4, name: 'Hannah', address: 'Mountain 21'},
  { id: 1, name: 'John', address: 'Higheay 71'},
  { id: 5, name: 'Michael', address: 'Valley 345'},
  { id: 2, name: 'Peter', address: 'Lowstreet 4'},
  { id: 8, name: 'Richard', address: 'Sky st 331'},
  { id: 6, name: 'Sandy', address: 'Ocean blvd 2'},
  { id: 9, name: 'Susan', address: 'One way 98'},
  { id: 10, name: 'Vicky', address: 'Yellow Garden 2'},
  { id: 14, name: 'Viola', address: 'Sideway 1633'},
  { id: 12, name: 'William', address: 'Central st 954'}
]
```

## 按订单排序

使用DESC关键字以降序对结果进行排序。

`按name的字母顺序对结果进行反向排序：`

```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  con.query("SELECT * FROM customers ORDER BY name DESC", function (err, result) {
    if (err) throw err;
    console.log(result);
  });
});
```
结果
```js
[
  { id: 12, name: 'William', address: 'Central st 954'},
  { id: 14, name: 'Viola', address: 'Sideway 1633'},
  { id: 10, name: 'Vicky', address: 'Yellow Garden 2'},
  { id: 9, name: 'Susan', address: 'One way 98'},
  { id: 6, name: 'Sandy', address: 'Ocean blvd 2'},
  { id: 8, name: 'Richard', address: 'Sky st 331'},
  { id: 2, name: 'Peter', address: 'Lowstreet 4'},
  { id: 5, name: 'Michael', address: 'Valley 345'},
  { id: 1, name: 'John', address: 'Higheay 71'},
  { id: 4, name: 'Hannah', address: 'Mountain 21'},
  { id: 13, name: 'Chuck', address: 'Main Road 989'},
  { id: 7, name: 'Betty', address: 'Green Grass 1'},
  { id: 11, name: 'Ben', address: 'Park Lane 38'},
  { id: 3, name: 'Amy', address: 'Apple st 652'}
]
```

# delete

## 删除记录
您可以使用“ DELETE FROM”语句从现有表中删除记录：
`删除地址为“ Mountain 21”的所有记录：`
```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  var sql = "DELETE FROM customers WHERE address = 'Mountain 21'";
  con.query(sql, function (err, result) {
    if (err) throw err;
    console.log("Number of records deleted: " + result.affectedRows);
  });
});
```
**请注意DELETE语法中的WHERE子句： WHERE子句指定应删除的记录。如果省略WHERE子句，则将删除所有记录！**

## 结果对象

执行查询时，将返回结果对象。

结果对象包含有关查询如何影响表的信息。

从上面的示例返回的结果对象如下所示：
```js
{
  fieldCount: 0,
  affectedRows: 1,
  insertId: 0,
  serverStatus: 34,
  warningCount: 0,
  message: '',
  protocol41: true,
  changedRows: 0
}
```
属性值可以这样显示
`
console.log(result.affectedRows)
`

# Drop Table

## 删除表格
您可以使用“ DROP TABLE”语句删除现有表：

`删除表“ customers”：`
```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  var sql = "DROP TABLE customers";
  con.query(sql, function (err, result) {
    if (err) throw err;
    console.log("Table deleted");
  });
});
```


## 仅在存在时drop
如果您要删除的表已被删除，或者由于任何其他原因不存在，则可以使用IF EXISTS关键字来避免出现错误。
`删除表“ customers”（如果存在）：`
```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  var sql = "DROP TABLE IF EXISTS customers";
  con.query(sql, function (err, result) {
    if (err) throw err;
    console.log(result);
  });
});
```

如果该表存在，则结果对象将如下所示：

```js
{
  fieldCount: 0,
  affectedRows: 0,
  insertId: 0,
  serverstatus: 2,
  warningCount: 0,
  message: '',
  protocol41: true,
  changedRows: 0
}
```

如果该表不存在，则结果对象将如下所示：
```js
{
  fieldCount: 0,
  affectedRows: 0,
  insertId: 0,
  serverstatus: 2,
  warningCount: 1,
  message: '',
  protocol41: true,
  changedRows: 0
}
```
如您所见，唯一的区别是如果表不存在，则warningCount属性设置为1。

# update

## update表
您可以使用“ UPDATE”语句来更新表中的现有记录：

`将地址列从“ Valley 345”覆盖为“ Canyon 123”：`
```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  var sql = "UPDATE customers SET address = 'Canyon 123' WHERE address = 'Valley 345'";
  con.query(sql, function (err, result) {
    if (err) throw err;
    console.log(result.affectedRows + " record(s) updated");
  });
});
```

**请注意UPDATE语法中的WHERE子句： WHERE子句指定应更新的记录。如果省略WHERE子句，所有记录将被更新！**

## 结果对象
执行查询时，将返回结果对象。

结果对象包含有关查询如何影响表的信息。

从上面的示例返回的结果对象如下所示
```js
{
  fieldCount: 0,
  affectedRows: 1,
  insertId: 0,
  serverStatus: 34,
  warningCount: 0,
  message: '(Rows matched: 1 Changed: 1 Warnings: 0',
  protocol41: true,
  changedRows: 1
}
```

# limit

## 限制结果
1.您可以使用“ LIMIT”语句来限制查询返回的记录数：

`在“customers”表中选择前5条记录：`
```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  var sql = "SELECT * FROM customers LIMIT 5";
  con.query(sql, function (err, result) {
    if (err) throw err;
    console.log(result);
  });
});
```

## 从另一个位置开始
2.如果要从第三条记录开始返回五条记录，则可以使用“ OFFSET”关键字：

`从位置3开始，并返回接下来的5条记录：`
```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  var sql = "SELECT * FROM customers LIMIT 5 OFFSET 2";
  con.query(sql, function (err, result) {
    if (err) throw err;
    console.log(result);
  });
});
```
注意： “offset 2”表示从第三个位置开始，而不是第二个位置！

## 较短的语法
3.您还可以使用像这样的“ LIMIT 2、5”这样编写SQL语句，该语句返回与上面的偏移量示例相同的结果：

`从位置3开始，并返回接下来的5条记录：`
```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  var sql = "SELECT * FROM customers LIMIT 2, 5";
  con.query(sql, function (err, result) {
    if (err) throw err;
    console.log(result);
  });
});
```

**注意：数字是相反的：“ LIMIT 2、5”与“ LIMIT 5 OFFSET 2”相同**

# JOIN

## 联接两个或更多表
您可以使用JOIN语句基于两个或多个表之间的相关列来合并行。

考虑您有一个“customers”表和一个“production”表：

使用者
```js
[
  { id: 1, name: 'John', favorite_product: 154},
  { id: 2, name: 'Peter', favorite_product: 154},
  { id: 3, name: 'Amy', favorite_product: 155},
  { id: 4, name: 'Hannah', favorite_product:},
  { id: 5, name: 'Michael', favorite_product:}
]
```
产品展示
```js
[
  { id: 154, name: 'Chocolate Heaven' },
  { id: 155, name: 'Tasty Lemons' },
  { id: 156, name: 'Vanilla Dreams' }
]
```
可以通过使用用户favorite_product字段和产品 id字段来组合这两个表。

`选择两个表中都匹配的记录：`
```js
var mysql = require('mysql');

var con = mysql.createConnection({
  host: "localhost",
  user: "yourusername",
  password: "yourpassword",
  database: "mydb"
});

con.connect(function(err) {
  if (err) throw err;
  var sql = "SELECT users.name AS user, products.name AS favorite FROM users JOIN products ON users.favorite_product = products.id";
  con.query(sql, function (err, result) {
    if (err) throw err;
    console.log(result);
  });
});
```

**注意：您可以使用INNER JOIN代替JOIN。他们都会给你相同的结果。**

## left JOIN
如果要返回所有用户，无论他们是否拥有喜欢的产品，请使用LEFT JOIN语句：

`选择所有用户及其喜爱的产品：`
```js
SELECT users.name AS user,
products.name AS favorite
FROM users
LEFT JOIN products ON users.favorite_product = products.id
```

## right JOIN
如果您想退回所有产品，并且将其作为收藏的用户，即使没有用户将其作为收藏，请使用RIGHT JOIN语句：

`选择所有产品以及将其作为收藏的用户：`
```js
SELECT users.name AS user,
products.name AS favorite
FROM users
RIGHT JOIN products ON users.favorite_product = products.id
```
结果
```js
[
  { user: 'John', favorite: 'Chocolate Heaven' },
  { user: 'Peter', favorite: 'Chocolate Heaven' },
  { user: 'Amy', favorite: 'Tasty Lemons' },
  { user: null, favorite: 'Vanilla Dreams' }
]
```
**注意：没有喜欢的产品的汉娜（Hannah）和迈克尔（Michael）不包括在结果中。**


