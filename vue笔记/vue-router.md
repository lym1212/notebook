# 前端路由

- hash 地址和组件的对应关系
- 点击路由链接，地址栏的 hash 值发生变化
- 前端路由监听到了地址变化，把 hash 地址对应的组件渲染到浏览器

# Vue Router

- 安装

  ```shell
  npm install vue-router@3.5.2 -S
  ```

- 创建路由  `router/index.js`

  ```javascript
  import Vue from 'vue'
  import VueRouter from 'vue-router'
  // 导入组件
  import Home from 'Home.vue'
  // 安装插件
  Vue.use(VueRouter)
  // 创建实例
  const router = new VueRouter({
      // 路由规则：定义路由和组件的对应关系
      routes: [
          { path: '/home', component: Home },
          // 重定向
          { path: '/', redirect: '/home' }
      ]
  })
  // 导出实例
  exports default router
  ```

- 挂载 `App.vue`

  ```javascript
  import router from 'router'
  
  new Vue({
      render: h => h(App)
      router
  }).$mount('#app')
  ```

# `<router-link>`

- 支持用户在有路由功能的应用中导航，`to` 属性指向目标地址
- 在浏览器中默认会渲染成 `<a>` 标签

```html
<!-- 声明式导航 -->
<router-link to="/home"></router-link>
<!-- active-class: 设置链接激活时使用的 CSS 类 -->
```

# `<router-view>`

- 占位符，渲染路径匹配的组件

- 命名视图

  ```html
  <router-view name="a"></router-view>
  ```

# 嵌套路由

- 通过路由加载出来的组件的模板中也有 `<router-view>`

```javascript
routes: [
    {
        path: '/user',
        component: Home,
        // 配置子路由规则
        chilren: [
            // 默认子路由
            { path: '', component: cpn }
            // '/user/profile'
            { path: 'profile', component: Profile }
        ]
    }
]
```

# 动态路由

```javascript
{ path: 'user/:id', component: User }
// User.vue 中获取 id
this.$route.params.id
```

#### 传参 

```javascript
// router
{ path: 'user/:id', component: User, props: true }

// User.vue
props: ['id']
```

# 编程式导航

>  js 中调用 `location.href`
>
>  声明式：<router-link :to="...">

```javascript
// 增加一条历史记录
this.$router.push('/xxx')  
// 替换历史记录
this.$router.replace('/xxx')
// 在历史记录中前进或后退，0 是当前记录
// 超过上限不会跳转
this.$router.go(n)
// go的简化方法
this.$router.back()
this.$router.forward()
```

> `this.$router` 就是 router 实例

# 导航守卫*

- 通过跳转或取消的方式守卫导航

#### 全局前置守卫

- 每次发生跳转的时候都会触发

```javascript
router.beforeEach((to, from, next) => {
    // to：将要进入的路由对象
    // from：正要离开的路由
    // next：next()，必须调用一次，表示放行
})
```

- `next()` 函数的调用方式

  ```javascript
  // 直接放行
  next()
  // 进行一个新的导航
  next('/xxx')
  // 中断当前的导航
  next(false)
  ```

#### 全局后置钩子

- `router.AfterEach((to, from) => { ... })`

# $route

- 路由参数对象，表示当前激活路由的状态信息
- 包括当前 URL 解析的信息和 URL 匹配到的路由记录
- 每次导航后都会产生一个新的路由对象

#### 属性

- `$route.name`：当前路由的名称（命名路由才有
- `$route.path`：当前路由的路径
- `$route.fullPath`：完整路径，包括查询参数和 hash
- `$route.params`：动态路由的路由参数
- `$route.query`：URL 查询参数
- `$route.hash`：当前路由的 hash 值（带 #
- `$route.matched`（Array：当前路由所有嵌套路径片段的路由记录（routes 数组中的对象副本
- `$route.redirectedForm`：重定向来源的路由的名字

# 路由懒加载

```javascript
const Foo = () => import('./Foo.vue')
// import() 函数返回一个Promise对象
// 使用Babel要安装插件才能解析：syntax-dynamic-import

// 分块
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
```

