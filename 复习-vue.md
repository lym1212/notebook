# 优点

- 低耦合
- 可重用性
- 独立开发
- 可测试

#  双向绑定原理

- 发布者-订阅者模式
- 使用`Object.defineProperty()`实现数据劫持，监听数据的 setter 和 getter
- 赋值时调用setter，取值时调用getter
- view更新data：监听input事件
- data更新view：监听属性的set

# 时光旅行调试

- 把记录中的state替换到当前的state
- `store.replaceState()`实现

# 给Vue实例中的data的对象属性响应式地添加新属性

```javascript
vm.$set(object，propertyName，value)
// 全局
Vue.set(object，propertyName，value)
// 数组也是
```

实现原理

- 如果是数组，用splice方法
- 如果是对象，属性存在直接赋值，不存在则进行响应式处理（`defineReactive()`

# SPA

- 只在页面初始化时加载html、js、css

优点

- 内容改变不用重新加载整个页面，避免不必要的跳转和重复渲染
- 服务器压力小，前端交互，后端数据处理

缺点

- 第一次加载时间久，部分页面是按需加载
- seo 难度大

# 单向数据流

- 父传子

# 计算属性

- 有缓存，发生改变时才会重新计算

# SSR 服务端渲染

优点

- 更好seo，搜索引擎可以抓取渲染好的页面
- 首次加载快，服务端渲染好页面返回，不用等待下载js文件

缺点

- 只支持beforeCreate和created
- 要处于nodejs server 环境
- 需要更多的服务器负载

# 路由模式

- hash：支持所有浏览器
  - hash值就是#后面的内容，`location.hash`
  - 发送请求时hash部分不会被发送
  - hash值改变，浏览器的访问历史会增加记录，通过浏览器的回退和前进可以控制hash的切换
  - `hashchange`事件可以监听hash值得变化，来跳转页面
- history：依赖history API 和服务端配置
  - history对象，表示当前窗口的浏览历史
  - `History.pushState()`和 `History.replaceState()`操作历史记录，不会触发`popstate`事件
  - `popstate` 事件监听url的变化
- abstract：支持所有js运行环境

# 虚拟DOM

- 保证性能下限，不用手动操作DOM，可以跨平台
- 实现原理：
  - js 对象模拟真实DOM树
  - diff算法比较两个虚拟DOM树的差异
  - pach算法将差异应用到真正的DOM树

# key

- 如果没有key，vue会最大程度复用元素
- 使用key时，表示元素是独立的，不要复用