> 专注于数据请求的库

## 基本语法

```javascript
// axios 的返回值是 promise 对象 
axios({
    method: 'get',
    url: '',
    params: {
        id: 777
    }
}).then(res => {
    console.log(res.data)
})
// post 请求
axios({
    method: 'post',
    url: '',
    data: {
        name: 'mjq'
    }
}).then(res => {
    console.log(res.data)
})
```

## 使用 async 函数

```javascript
async function() {
    // 解构赋值，data 是匹配模式，res 是变量
    const { data: res } = await axios({})
    console.log(res)
}
```

## 请求方法

- GET 请求

```javascript
const { data: res } = await axios.get(url, { params: { id: 77 } })
```

- POST 请求

```javascript
const { data: res } = await axios.post(url, { name: 'mjq' })
```
## axios 挂载到 Vue 原型上

> 缺点：不能实现接口复用

```javascript
import axios from 'axios'

// 配置根路径
axios.default.baseURL = ''
// 挂载到原型上
Vue.prototype.$http = axios
// 使用
this.$http.get()
```

