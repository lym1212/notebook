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

##  模板引擎

```shell
npm install --save art-template
npm install --save express-art-template
```

- 配置：

  ```javascript
  app.engine('html', require('express-art-template'));
  ```


## 解析post数据

```shell
npm install body-parser --save 
```

- 配置：

  ```javascript
  app.use(bodyParser.urlencoded({ extended: false }))
  app.use(bodyParser.json())
  ```

- 获取数据：`req.body`

## 创建路由

```javascript
const router = express.Router()

router.get('/', (req, res) => {
    
})
router.post('/xxx', (req, res) => {
    
})
```

