# 概念

- 一个 构建用户界面 的 前端框架

- vue 的特性：
  - 数据驱动视图：页面依赖的数据改变，页面会自动重新渲染（单向的数据绑定）
  - 双向数据绑定：填写表单时，自动把填写的内容同步到数据

#### MVVM

> Model、View、ViewModel

- Model：页面渲染时依赖的数据
- View：页面渲染的 DOM 结构
- ViewModel：vue 的实例，MVVM 核心，把 Model 和 View 连接在一起

# Vue 实例

```javascript
new Vue({
    // el 属性和 new Vue().$mount('#app') 作用相同
    el: '#app',
    // 属性
    data,
    computed,
    watch,
    methods,
    filters,   // Vue3 删除
    components,
    darectives    // 自定义指令
})
```

# Vue指令

- vue 的模板语法，辅助渲染页面的基本结构
- 用途划分：内容渲染、属性绑定、事件绑定、双向绑定、条件渲染、列表渲染

#### 1. 内容渲染

- `<p v-text="name"></p>`：会覆盖标签原文本的内容

- `<p>{{ name }}</p>`：Mustache 语法，插值表达式，内容占位符，不会覆盖原文本

  > 可以做简单运算和调用方法

- `<p v-html="info"></p>`：如果 info 是 html 代码字符串，会被解析并渲染

#### 2. 属性绑定

- `<input v-bind:placeholder="tips">`：动态绑定属性，简写 `:placeholder`

  > 双引号内可以做简单运算和字符串拼接

#### 3. 事件绑定

- `<button v-on:click="add">+1</button>`：事件监听，简写 `@click`

  > $event 可以获取事件对象：`add($event, 1)`

- 事件修饰符：
  - `@click.prevent`：作用等于原生 js 的 `e.preventDefault()`
  - `@click.stop`：作用等于原生 js 的 `e.stopPropagation()`
  - `@click.native`

- 按键修饰符：
  - `@keyup.enter`：只监听回车键
  - `@keyup.esc`：只监听退出键

#### 4. 双向绑定

- `<input type="text" v-model="name">`：本质是 v-bind 和 @input

  ```javascript
  <input :value="msg" @input="msg = $event.target.value">
  ```

- 一般用于 input 标签，复选框，select 标签

- v-model 修饰符
  - `v-model.lazy`：回车或者失去焦点才会更新数据
  - `v-model.number`：把输入的值转换为数值
  - `v-model.trim`：过滤首尾空格

#### 5. 条件渲染

- `v-if`：是否渲染该元素，动态删除或创建元素

  > v-if    v-else-if    v-else 

- `v-show`：是否隐藏该元素，频繁切换时使用，相当于 `display:none;`

#### 6. 列表渲染

```html
<li v-for="(item, index) in list" :key="item.id">{{ item.name }}</li>
```

> key 只能是数值或者字符串且要具有唯一性，index 没有唯一性

# 过滤器 (Vue3删除)

- 可以用在 Mustache 语法和 v-bind 表达式中，用管道符( | )调用

  ```html
  {{ message | capitalize }}
  <div v-bind:id="rawId | formatId"></div>
  ```

- 全局过滤器

  ```javascript
  Vue.filter('xxxxx', val => {
      return 'xxxxxxxxx'
  })
  ```

# 侦听器

- 监听数据的变化并响应操作

  ```javascript
  data() {
    return {
        question: 'xxx',
        info: {
            username: 'xxxxx'
        }
    }  
  },
  watch: {
      // 函数写法，方法名是监听的数据名
      // 刚进入页面时不会触发
      question(newVal, oldVal) {}
      
      // 对象写法，设置 immediate 可以自动触发
      question: {
          handler(newVal, oldVal) {},
          // 进入页面时侦听器会自动触发一次，默认 false
          immediate: true
      },
      info: {
          handler(newVal, oldVal) {},
          // 深度侦听，可以侦听对象中所有属性的变化
          deep: true
      },
      // 侦听对象属性
      'info.username'(newVal, oldVal) {}
  }

# 组件

- UI 结构的复用
- 构成部分：template，script，style

#### 注册全局组件

```javascript
// main.js 
import xxx from ./components/xxx
Vue.component('xxx', xxx)
```

#### 组件之间的样式冲突

```vue
// 给 style 标签添加 scoped 属性
// 原理：给同一个组件中的标签添加同一个属性和属性选择器
<style scoped>
</style>
// 父组件改变子组件的样式，在选择器前面加 /deep/
// 一般用第三方组件时使用
/deep/ h5 {
}
```

#### 组件的生命周期

- 一个组件从创建、运行到销毁的阶段

> 创建阶段：new Vue()  ->  beforeCreate()  ->  created  ->  beforeMount()  ->  mounted

- 创建实例对象（new Vue()，初始化事件和生命周期函数（数据和方法还未被创建（beforeCreate
- 初始化数据和方法，组件模板结构没有生成（created（一般在 created 函数中发送请求
- 根据数据和模板编译生成 HTML 结构（beforeMount（浏览器中还没有渲染，不能操作 dom
- 创建组件的 $el 属性，把生成的 HTML 结构替换给 el 属性指定的 dom 元素（mounted
- 浏览器中已经渲染了当前组件的 dom 结构，可以操作 dom

> 运行阶段：->  beforeUpdate()  ->  updated()

- 数据发生变化（beforeUpdate（即将根据变化过的数据重新渲染模板结构
- 根据最新的数据重新渲染组件的 dom 结构（updated（完成渲染

> 销毁阶段：->  beforeDestroy()  ->  destroyed()

- beforeDestroy（即将销毁，还在正常工作
- 销毁当前组件的侦听器、子组件、事件监听器（destroyed
- 该组件在浏览器中的 dom 结构已被移除

#### 组件的数据共享 

##### 1. 父传子

- 父组件中通过自定义属性传递数据，子组件通过 props 属性接收数据

- 传递对象传递的是对象的引用，在子组件修改对象的属性，父组件的值也会改变

- props 属性的数据可以和 data 一样使用，但不建议修改

  ```vue
  // age 前面不加: 传的是字符串
  <p :age="17"></p>   =>   props: ['age']
  ```

- props 写法

  ```javascript
  props: {
      name: String
  }
  props: {
      name: {
          type: String,
          default: 'mjq',
          required: true
      }
  }
  ```

##### 2. 子传父

- 子组件通过 `this.$emit()` 触发自定义事件，父组件监听自定义事件接收数据

  > 当父组件传给子组件的 xxx prop，子组件要改变这个值时，子组件要通过发送 'update:xxx' 来更新，父组件再监听 update:xxx 获取更新
  >
  > 这个方法的简写是：:xxx.sync="xxxxx"（模拟双向绑定
  
  ```vue
  // 子组件 Son.vue
  this.$emit('eventname', this.sdata)
  
  // 父组件
  <son @eventname="methodname"></son>
  // 接收
  methodname(val) {
  	this.fdata = val
  }
  ```

##### 3. 兄弟组件

- 事件总线（EventBus

  ```javascript
  // eventBus.js
  import Vue from 'vue'
  export default new Vue()
  
  // A.vue 发送
  import eventBus from 'eventBus.js'
  eventBus.$emit('event', data)
  
  // B.vue 接收
  import eventBus from 'eventBus.js'
  // created 或者 mounted 函数中监听
  eventBus.$on('event', val => {
  	this.data = val
  })
  ```

- 页面销毁时移除

  ```javascript
  eventBus.$off('event')
  ```

- 全局创建 EventBus

  ```javascript
  Vue.prototype.$bus = new Vue()
  // 使用：this.$bus.$emit
  // this.$bus.$on / this.$bus.$off
  ```
#### ref 引用

- 给标签或者组件添加 ref 属性

  ```vue
  <p ref="pro"></p>
  // 获取该标签的 dom 结构
  this.$ref.pro
  ```
# 动态组件

#### 动态组件渲染

- 内置的 `<component>` 组件，作用是组件的占位符

- 动态绑定 `is` 属性，属性值是哪个组件名就会渲染哪个组件

  ```vue
  <component :is="cptName"></component>
  ```

#### keep-alive 标签

- 把组件添加到 `<keep-alive` 标签中，切换时组件的状态会保持，组件不会被销毁

- 切换时会执行 `activated` 和 `deactivated` 生命周期函数

- 属性

  - include：要缓存的组件（name 属性 | 注册时的名字

  - exclude：不缓存的组件（name 属性 | 注册时的名字

    > 注册时的名字：以标签的形式使用，name 属性：结合 keep-alive 使用、调试工具中显示的名称

  - max：最多缓存多少组件实例，超过限制会被销毁

- 父组件调用子组件方法

  ```vue
  this.$ref.son.method()
  ```

# 插槽

- 使用组件时可以自定义内容的部分

- 如果省略 name 属性，默认为 default

#### 默认内容

```html
<slot name="test">default content</slot>
```

> 使用时没有指定内容会显示默认内容，指定了就会覆盖

#### 具名插槽

- 有 name 属性，使用 `v-slot` 指令

```html
<slot name="test"></slot>
<!-- 使用 -->
<!-- v-slot 只能用在 <template> 标签，简写：#test -->
<template v-slot:test></template>
```

#### 作用域插槽

```html
<!-- 给插槽绑定属性传给父组件使用，这个属性叫插槽 prop -->
<slot :user="user">{{ user.name }}</slot>
<!-- slotProps 是一个对象，包括了所有插槽 prop -->
<template v-slot:default="slotProps">
    {{ slotProps.user.name }}
</template>
```

- 只有默认插槽时，`v-slot` 可以直接写在组件标签上

  ```html
  <cpn v-slot:default="slotProps"></cpn>
  <!-- 简写 -->
  <cpn v-slot="slotProps"></cpn>
  ```

  > 不能和具名插槽混用，有多个插槽要使用 `<template>` 的语法

- 解构赋值

  ```html
  <template v-slot:default="{ user }">
      {{ user.name }}
  </template>
  <!-- 重命名 -->
  <template v-slot:default="{ user: person }">
      {{ person.name }}
  </template>
  <!-- 设置默认值 -->
  <template v-slot:default="{ user = { name: 'majiaqi' } }">
      {{ user.name }}
  </template>
  ```

# 自定义指令

- 全局自定义指令和局部自定义指令

  ```javascript
  // 全局
  Vue.directive('focus', {
      inserted(el) {
          el.focus()
      }
  })
  // 私有
  directives: {
      focus: {
          inserted(el) {
              el.focus()
          }
      }
  }
  // 使用
  // <input v-focus>
  ```

#### 钩子函数

> 都是可选项

- `bind`：指定第一次绑定时调用，只调用一次（初始化
- `unbind`：指令解绑时调用，只调用一次
- `inserted`：被绑定元素插入父节点时调用（不一定被插入文档
- `update`：每一次 dom 更新的时候调用
- `componentUpdated`：

#### 参数

> 钩子函数之间数据共享通过 `dataset`

- `el`：指令绑定的元素，可以操作 dom
- `binding`：一个对象，包括以下属性
  - `name`：指令名称
  - `value`：指令的绑定值
  - `oldValue`：指令绑定的上一个值
  - `expression`：指令绑定值的字符串形式（不是值
  - `arg`：传给指令的参数（ `v-color:foo`
  - `modifiers`：包含指令修饰符的对象
- `vnode`：
- `oldVnode`：

#### 函数简写

- 如果 `bind` 和 `update` 函数的逻辑相同，可以简写

  ``` javascript
  // 全局
  Vue.directive('color', function (el, binding) {
    el.style.color = binding.value
  })
  // 私有
  directives: {
      color(el, binding) {
          el.style.color = binding.value
      }
  }
  ```

  











# 实例方法

#### 生命周期

1. `vm.$nextTick(cb)`：延迟回调函数中的代码到下次 dom 更新之后执行
   - 修改数据后立即使用，然后等待 dom 更新
   - 回调的 this 自动绑定到调用该方法的实例上
   - 如果没有提供回调函数，返回一个 Promise

