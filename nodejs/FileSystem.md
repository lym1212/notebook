## 引入

```
const fs = require('fs')
```

## 读取文件

```
fs.readFile(path, callback)   // 异步
```

- path： 文件路径（如果是相对路径，是相对于执行node命令时的路径）

- 回调函数参数： 
  - error
  - data(一个Buffer对象)，`toString`转换

## 写入文件

```
fs.writeFile(path, data, callback)
```

- 文件路径
- 要写的内容
- 回调函数参数： error









- 打开文件：`fs.openSync('test.txt', 'w')`     // 返回一个文件描述符

  

