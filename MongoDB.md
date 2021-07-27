## MongoDB

- 最像关系型数据库的非关系型数据库
- 一个数据库可以有多个集合（表）
- 一个集合可以有多个文档（表记录）
- 文档结构灵活

```shell
// 启动
// 默认目录 /data/db
mongod

// 链接
mongo

// 退出
exit
```

```
// 显示所有数据
show dbs
// 显示当前数据库对象
db
// 不存在则创建，存在则连接到指定数据库
use xxx
// 插入数据
db.xxx.insert({"xxx":"xxxx"})
// 删除数据库 先连接到要删除的数据库
db.dropDatabase()
// 创建集合
db.createCollection("xxx")
// 删除集合(表)
db.xxx.drop()
```

| RDBMS  | MongoDB  |
| ------ | -------- |
| 表格   | 集合     |
| 行     | 文档     |
| 列     | 字段     |
| 表联合 | 嵌入文档 |
| 主键   | _id      |

## mongoose

#### 安装

```shell
npm install mongoose
```

#### 使用

```javascript
const mongoose = require('mongoose');
// 连接数据库 不存在则在插入第一条数据后创建
mongoose.connect('mongodb://localhost/test');

const Cat = mongoose.model('Cat', { name: String });

const kitty = new Cat({ name: 'Zildjian' });
kitty.save().then(() => console.log('meow'));
```

#### 设计文档结构

```javascript
var userSchema = new mongoose.Schema({
	username: {
	  type: String,
      required: true
	},
    password: {
      type: String,
      required: true
    },
    email: String
})
```

- 合法的数据类型：String, Number, Date, Buffer, Boolean, Mixed, Objected, Array, Decimal128

#### 发布为模型

```javascript
// User(数据库名字) => users(集合名字)
// 返回模型构造函数
var User = mongoose.model('User', userSchema)
```

#### 实例化

```javascript
var admin = new User({
	username: 'admin',
	password: '123456',
	email: 'admin@admin.com'
})
// 保存到数据库
admin.save((err, res) => {
    if(err) return console.log('保存失败')
})
```

#### 获取数据

```javascript
// 查询所有
User.find((err, res) => {
	if(err) return console.log('查询失败')
    // 打印查询到的结果
    console.log('res')
})
```

