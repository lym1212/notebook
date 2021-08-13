## Node.js

* 一个 JavaScript 运行时环境，基于 Chrome 的 V8 引  擎构建
* 可以解析和执行 JavaScript 代码
* 使 JavaScript 可以脱离浏览器运行
* Node.js 中的 JavaScript：文件读写、网络通信、http服务器构建  
* 使用事件驱动和非阻塞 I/O 模型

## Node中使用模板引擎

```javascript
const template = require('art-template')   // 引入
const ret = template.render(data, {})
```

- 服务端渲染：使用模板引擎

- 客户端渲染：不利于SEO优化，异步加载很难被爬虫抓取 (例如：商品评论)

- art-template中遍历数组：

  ```
  {{each Array}}
  	<li>{{ $value }}</li>
  {{/each}}
  ```

- 状态码：
  - 301：永久重定向，浏览器会记忆
  - 302：临时重定向

## nodemon自动重启服务器

```shell
npm install nodemon -g
nodemon app.js
```

## node中的其他成员

- `__dirname`：当前模块的目录名
- `__filename`：当前模块的绝对路径（包括文件名）

















## 命令行窗口、cmd、shell、终端、DOS

- dir: 当前目录所有文件
- md: 创建一个文件夹
- rd: 删除文件夹
- 打开文件: 输入文件名 (把路径添加到path可以在任何目录打开)

#### 环境变量(windows系统中的变量)



