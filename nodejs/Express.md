## 使用
```javascript
const express = require('express')

const app = express()
// 公开指定目录
// 如果省略第一个参数，可省略 /public 直接访问该目录下的文件
app.use('/public/', express.static('./public'))
// 判断路径
app.get('/home', (req, res) => {
    // get 数据
    req.query
    res.send('XXXXXX')
})

app.listen(3000, () => {
    console.log('running...')
})
```

## 重定向

```
res.redirect('/')
```

## 创建路由

```javascript
// router.js
const router = express.Router()

router.get('/', (req, res) => {    
})
router.post('/xxx', (req, res) => {    
})
// 导出
module.exports = router

// app.js 挂载
const router = require('./router.js')
app.use(router)
```

## 发送JSON响应

- 把对象转为字符串

```javascript
res.json(null)
res.json({ user: 'tobi' })
res.status(500).json({ error: 'message' })
```

##  模板引擎

#### art-template

```shell
npm install --save art-template
npm install --save express-art-template
```

- 配置：

  ```javascript
  app.engine('html', require('express-art-template'));
  ```

## 中间件

#### body-parser: 解析post数据

```shell
npm install body-parser --save 
```

- 引入

  ```javascript
  const bodyParser = require('body-parser')
  ```

- 配置

  ```javascript
  // 路由挂载之前配置
  app.use(bodyParser.urlencoded({ extended: false }))
  app.use(bodyParser.json())
  ```

- 获取数据：`req.body`

#### express-session

```shell
npm install express-session
```

- 配置

  ```javascript
  app.use(session({
    secret: 'keyboard cat',
    resave: false,
    saveUninitialized: true
  }))
  ```

  
