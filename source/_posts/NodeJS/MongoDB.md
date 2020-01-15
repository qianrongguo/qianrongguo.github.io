    cnpm install mongodb

### 创建数据库
   要在MongoDB中创建数据库，请先创建一个MongoClient对象，然后使用正确的IP地址和要创建的数据库名称指定连接URL。
   
   如果数据库不存在，MongoDB将创建该数据库并建立连接。
   
   创建mydb数据库:
   ```js
    var MongoClient = require('mongodb').MongoClient;
    var url = "mongodb://localhost:27017/mydb";
    
    MongoClient.connect(url, function(err, db) {
      if (err) throw err;
      console.log("Database created!");
      db.close();
    });
```
 **重要提示：在MongoDB中，直到获得内容才创建数据库**
  
  在实际创建数据库（和集合）之前，MongoDB会等到您创建了一个集合（表）（至少包含一个文档（记录））后，再进行创建。
### 创建集合
**MongoDB中的集合与MySQL中的表相同**

要在MongoDB中创建集合，请使用createCollection()方法：

创建一个名为“客户”的集合：
```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  dbo.createCollection("customers", function(err, res) {
    if (err) throw err;
    console.log("Collection created!");
    db.close();
  });
});
```
**重要提示：在MongoDB中，只有在获得内容后才创建集合！**

MongoDB等到您插入文档后才真正创建集合。
### 数据库操作
-  insert

    #### 插入集合
             
   要将记录或在MongoDB中调用的文档插入集合，我们使用insertOne（）方法。

   **MongoDB中的文档与MySQL中的记录相同**
   insertOne（）方法的第一个参数是一个对象，其中包含您要插入的文档中每个字段的名称和值。
   它还带有一个回调函数，您可以在其中处理任何错误或插入结果：例


   将文档插入“customs”集合中：
   ```js
    var MongoClient = require('mongodb').MongoClient;
    var url = "mongodb://localhost:27017/";
    
    MongoClient.connect(url, function(err, db) {
      if (err) throw err;
      var dbo = db.db("mydb");
      var myobj = { name: "Company Inc", address: "Highway 37" };
      dbo.collection("customers").insertOne(myobj, function(err, res) {
        if (err) throw err;
        console.log("1 document inserted");
        db.close();
      });
    });
```
   **注意：如果您尝试在不存在的集合中插入文档，MongoDB将自动创建集合。**
   
   #### 插入多个文档
   
   要将多个文档插入MongoDB的集合中，我们使用insertMany()方法。
   
   insertMany()方法的第一个参数是一个对象数组，其中包含要插入的数据。
   
   它还带有一个回调函数，您可以在其中处理任何错误或插入结果：
   
   在"customrs"集合插入多个文档
   ```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  var myobj = [
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
  ];
  dbo.collection("customers").insertMany(myobj, function(err, res) {
    if (err) throw err;
    console.log("Number of documents inserted: " + res.insertedCount);
    db.close();
  });
});
```

   #### 结果对象
   执行insertMany（）方法时，将返回结果对象。
   结果对象包含有关插入如何影响数据库的信息
   从上面的示例返回的对象如下所示：
   ```js
{
  result: { ok: 1, n: 14 },
  ops: [
    { name: 'John', address: 'Highway 71', _id: 58fdbf5c0ef8a50b4cdd9a84 },
    { name: 'Peter', address: 'Lowstreet 4', _id: 58fdbf5c0ef8a50b4cdd9a85 },
    { name: 'Amy', address: 'Apple st 652', _id: 58fdbf5c0ef8a50b4cdd9a86 },
    { name: 'Hannah', address: 'Mountain 21', _id: 58fdbf5c0ef8a50b4cdd9a87 },
    { name: 'Michael', address: 'Valley 345', _id: 58fdbf5c0ef8a50b4cdd9a88 },
    { name: 'Sandy', address: 'Ocean blvd 2', _id: 58fdbf5c0ef8a50b4cdd9a89 },
    { name: 'Betty', address: 'Green Grass 1', _id: 58fdbf5c0ef8a50b4cdd9a8a },
    { name: 'Richard', address: 'Sky st 331', _id: 58fdbf5c0ef8a50b4cdd9a8b },
    { name: 'Susan', address: 'One way 98', _id: 58fdbf5c0ef8a50b4cdd9a8c },
    { name: 'Vicky', address: 'Yellow Garden 2', _id: 58fdbf5c0ef8a50b4cdd9a8d },
    { name: 'Ben', address: 'Park Lane 38', _id: 58fdbf5c0ef8a50b4cdd9a8e },
    { name: 'William', address: 'Central st 954', _id: 58fdbf5c0ef8a50b4cdd9a8f },
    { name: 'Chuck', address: 'Main Road 989', _id: 58fdbf5c0ef8a50b4cdd9a90 },
    { name: 'Viola', address: 'Sideway 1633', _id: 58fdbf5c0ef8a50b4cdd9a91 } ],
  insertedCount: 14,
  insertedIds: [
    58fdbf5c0ef8a50b4cdd9a84,
    58fdbf5c0ef8a50b4cdd9a85,
    58fdbf5c0ef8a50b4cdd9a86,
    58fdbf5c0ef8a50b4cdd9a87,
    58fdbf5c0ef8a50b4cdd9a88,
    58fdbf5c0ef8a50b4cdd9a89,
    58fdbf5c0ef8a50b4cdd9a8a,
    58fdbf5c0ef8a50b4cdd9a8b,
    58fdbf5c0ef8a50b4cdd9a8c,
    58fdbf5c0ef8a50b4cdd9a8d,
    58fdbf5c0ef8a50b4cdd9a8e,
    58fdbf5c0ef8a50b4cdd9a8f
    58fdbf5c0ef8a50b4cdd9a90,
    58fdbf5c0ef8a50b4cdd9a91 ]
}
```
属性值可以这样显示：

   返回插入的文档数：
   ```js
console.log(res.insertedCount)
```


   #### _id字段
   如果您未指定_id字段，则MongoDB将为您添加一个，并为每个文档分配唯一的ID。
   
   在上面的示例中，没有指定_id字段，从结果对象可以看到，MongoDB为每个文档分配了唯一的_id。

   如果确实指定_id字段，则每个文档的值必须唯一：
   
  在带有指定_id字段的“products”表中插入三个记录：
```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  var myobj = [
    { _id: 154, name: 'Chocolate Heaven'},
    { _id: 155, name: 'Tasty Lemon'},
    { _id: 156, name: 'Vanilla Dream'}
  ];
  dbo.collection("products").insertMany(myobj, function(err, res) {
    if (err) throw err;
    console.log(res);
    db.close();
  });
});
```

   #### Find
   
   **在MongoDB中，我们使用find和findOne方法在集合中查找数据。就像SELECT语句用于在MySQL数据库的表中查找数据一样。**
    
   ##### Find One
   要从MongoDB中的集合中选择数据，我们可以使用findOne（）方法。
   
   findOne（）方法返回选择中的第一个匹配项。

   findOne（）方法的第一个参数是查询对象。在此示例中，我们使用一个空的查询对象，该对象选择集合中的所有文档（但仅返回第一个文档）。

在客户集合中找到第一个文档：
```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  dbo.collection("customers").findOne({}, function(err, result) {
    if (err) throw err;
    console.log(result.name);
    db.close();
  });
});
```

   #### Find All
   要从MongoDB中的表中选择数据，我们还可以使用find（）方法。
   
   find（）方法返回选择中的所有匹配项。
   
   find（）方法的第一个参数是查询对象。在此示例中，我们使用一个空的查询对象，该对象选择集合中的所有文档。
   
   
   查找customs集合中的所有文档：
   ```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  dbo.collection("customers").find({}).toArray(function(err, result) {
    if (err) throw err;
    console.log(result);
    db.close();
  });
```

   #### Find Some
   
   find（）方法的第二个参数是投影对象，它描述要在结果中包括哪些字段。
   
   此参数是可选的，如果省略，则所有字段都将包含在结果中。
   
   返回客户集合中所有文档的“名称”和“地址”字段：
   ```js
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  dbo.collection("customers").find({}, { projection: { _id: 0, name: 1, address: 1 } }).toArray(function(err, result) {
    if (err) throw err;
    console.log(result);
    db.close();
  });
});
```
   **您不允许在同一对象中同时指定0和1值（除非其中一个字段是_id字段）。如果您指定值为0的字段，则所有其他字段的值为1，反之亦然：**
   
  此示例将从结果中排除“地址”：
  ```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  dbo.collection("customers").find({}, { projection: { address: 0 } }).toArray(function(err, result) {
    if (err) throw err;
    console.log(result);
    db.close();
  });
});
```
**要排除_id字段，必须将其值设置为0：**

如果在同一对象中同时指定0和1值，则会出现错误（除非其中一个字段是_id字段）：
```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  dbo.collection("customers").find({}, { projection: { name: 1, address: 0 } }).toArray(function(err, result) {
    if (err) throw err;
    console.log(result);
    db.close();
  });
});
```

#### 结果对象
从上面示例的结果可以看出，可以将结果转换为包含每个文档作为对象的数组。要返回例如第三个文档的地址，只需引用第三个数组对象的address属性：

返回第三个文档的地址：
```js
console.log(result[2].address);
```

### query

#### Filter 结果

在集合中查找文档时，可以使用查询对象过滤结果。

find（）方法的第一个参数是查询对象，用于限制搜索。

查找address“ Park Lane 38”的文档：
```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  var query = { address: "Park Lane 38" };
  dbo.collection("customers").find(query).toArray(function(err, result) {
    if (err) throw err;
    console.log(result);
    db.close();
  });
});
```

#### 用正则表达式过滤
您可以编写正则表达式以准确查找要搜索的内容。

**正则表达式只能用于查询字符串。**

要仅查找“address”字段以字母“ S”开头的文档，请使用正则表达式/ ^ S /：

查找地址以字母“ S”开头的文档：
```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  var query = { address: /^S/ };
  dbo.collection("customers").find(query).toArray(function(err, result) {
    if (err) throw err;
    console.log(result);
    db.close();
  });
});
```

#### 排序结果

使用sort（）方法对结果进行升序或降序排序。

sort（）方法采用一个参数，这是一个定义排序顺序的对象。

按名称的字母顺序对结果进行排序：
```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  var mysort = { name: 1 };
  dbo.collection("customers").find().sort(mysort).toArray(function(err, result) {
    if (err) throw err;
    console.log(result);
    db.close();
  });
});
```

#### 降序排列
在排序对象中使用值-1进行降序排序。

```js
{ name: 1 } // ascending
{ name: -1 } // descending
```

按名称的字母顺序对结果进行反向排序：
```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  var mysort = { name: -1 };
  dbo.collection("customers").find().sort(mysort).toArray(function(err, result) {
    if (err) throw err;
    console.log(result);
    db.close();
  });
});
```

#### delete

#### 删除文件

要删除在MongoDB中调用的记录或文档，我们使用deleteOne（）方法。

deleteOne（）方法的第一个参数是一个查询对象，用于定义要删除的文档

**注意：如果查询找到多个文档，则仅删除第一次出现的文档。**

删除address "Mountain 21" 的文档：
```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  var myquery = { address: 'Mountain 21' };
  dbo.collection("customers").deleteOne(myquery, function(err, obj) {
    if (err) throw err;
    console.log("1 document deleted");
    db.close();
  });
});
```

#### 删除许多
要删除多个文档，请使用deleteMany（）方法。

deleteMany（）方法的第一个参数是一个查询对象，用于定义要删除的文档。

删除地址以字母“ O”开头的所有文档：
```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  var myquery = { address: /^O/ };
  dbo.collection("customers").deleteMany(myquery, function(err, obj) {
    if (err) throw err;
    console.log(obj.result.n + " document(s) deleted");
    db.close();
  });
});
```

#### 结果对象

deleteMany（）方法返回一个对象，该对象包含有关执行如何影响数据库的信息。

大多数信息不是很重要，但该对象内部的一个对象称为“结果”，它告诉我们执行是否正常以及有多少文档受到影响

结果对象如下所示：
```json
{ n: 2, ok: 1 }
```
您可以使用此对象返回已删除文档的数量：

返回已删除文档的数量：
```js
console.log(obj.result.n);
```

### drop集合
您可以使用drop（）方法删除表或在MongoDB中调用的集合。

drop（）方法采用一个回调函数，该函数包含错误对象和result参数，如果成功删除了集合，则返回true，否则返回false。

删除"customs"表：
```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  dbo.collection("customers").drop(function(err, delOK) {
    if (err) throw err;
    if (delOK) console.log("Collection deleted");
    db.close();
  });
});
```

#### db.dropCollection

您也可以使用dropCollection（）方法删除表（集合）。

dropCollection（）方法采用两个参数：集合的名称和回调函数。

使用dropCollection（）删除“customs”集合：
```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  dbo.dropCollection("customers", function(err, delOK) {
    if (err) throw err;
    if (delOK) console.log("Collection deleted");
    db.close();
  });
});
```

#### 更新文件

您可以使用updateOne（）方法更新记录或在MongoDB中调用的文档。

updateOne（）方法的第一个参数是一个查询对象，用于定义要更新的文档。

**注意：如果查询找到多个记录，则仅更新第一个记录。**

第二个参数是定义文档新值的对象。

使用address="Valley 345"将文档更新为name ="Mickey"和address ="Canyon 123"：

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://127.0.0.1:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  var myquery = { address: "Valley 345" };
  var newvalues = { $set: {name: "Mickey", address: "Canyon 123" } };
  dbo.collection("customers").updateOne(myquery, newvalues, function(err, res) {
    if (err) throw err;
    console.log("1 document updated");
    db.close();
  });
});
```

#### 仅更新特定字段

使用`$set`运算符时，仅更新指定的字段：


将address ="Valley 345"更新为"Canyon 123"：

```js
var myquery = { address: "Valley 345" };
  var newvalues = { $set: { address: "Canyon 123" } };
  dbo.collection("customers").updateOne(myquery, newvalues, function(err, res) {
```

#### 更新许多文件

要更新所有符合查询条件的文档，请使用updateMany（）方法。

更新名称以字母“ S”开头的所有文档：
```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://127.0.0.1:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  var myquery = { address: /^S/ };
  var newvalues = {$set: {name: "Minnie"} };
  dbo.collection("customers").updateMany(myquery, newvalues, function(err, res) {
    if (err) throw err;
    console.log(res.result.nModified + " document(s) updated");
    db.close();
  });
});
```

#### 结果对象
updateOne（）和updateMany（）方法返回一个对象，该对象包含有关执行如何影响数据库的信息。

大多数信息并不是很重要，但对象内部的一个对象称为“结果”，它告诉我们执行是否正常以及受影响的文档数量。

结果对象如下所示：
```js
{ n: 1, nModified: 2, ok: 1 }
```

您可以使用此对象返回更新的文档数：

返回更新的文档数：
```js
console.log(res.result.nModified);
```

### limit

#### 限制结果

为了限制MongoDB中的结果，我们使用limit（）方法。

limit（）方法采用一个参数，一个数字定义要返回的文档数。


考虑您有一个"customs"集合：

将结果限制为仅返回5个文档：
```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  dbo.collection("customers").find().limit(5).toArray(function(err, result) {
    if (err) throw err;
    console.log(result);
    db.close();
  });
});
```


### join

#### join集合

MongoDB不是关联数据库，但是您可以使用`$lookup`阶段执行左外部联接。

通过`$lookup`阶段，您可以指定要与当前集合一起加入的集合以及应该匹配的字段。

考虑您有一个"orders"集合和一个"products"集合：

   orders
    
    [
      { _id: 1, product_id: 154, status: 1 }
    ]
    
  products
  
       [
         { _id: 154, name: 'Chocolate Heaven' },
         { _id: 155, name: 'Tasty Lemons' },
         { _id: 156, name: 'Vanilla Dreams' }
       ]
       
       

将匹配的“products”文档加入“orders”集合：

```js
var url = "mongodb://127.0.0.1:27017/";

MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  var dbo = db.db("mydb");
  dbo.collection('orders').aggregate([
    { $lookup:
       {
         from: 'products',
         localField: 'product_id',
         foreignField: '_id',
         as: 'orderdetails'
       }
     }
    ]).toArray(function(err, res) {
    if (err) throw err;
    console.log(JSON.stringify(res));
    db.close();
  });
});
```

从上面的结果可以看到，产品集合中的匹配文档以数组的形式包含在订单集合中。






    
