---
date: 2021-5-25
tag:
  - Node
  - MySQL
author: liningjiang
location: Chengdu
---

# 使用 Node 操作 MySQL 数据库

文档：https://www.npmjs.com/package/mysql

安装：

```shell
npm install --save  mysql
```

```javascript
// 引入mysql包
var mysql = require("mysql");

// 创建连接
var connection = mysql.createConnection({
  host: "localhost", //本机
  user: "me", //账号root
  password: "secret", //密码12345
  database: "my_db", //数据库名
});

// 连接数据库	（打开冰箱门）
connection.connect();

//执行数据操作	（把大象放到冰箱）
connection.query("SELECT * FROM `users` ", function(error, results, fields) {
  if (error) throw error; //抛出异常阻止代码往下执行
  // 没有异常打印输出结果
  console.log("The solution is: ", results);
});

//关闭连接	（关闭冰箱门）
connection.end();
```
