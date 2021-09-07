- 状态管理，管理所有组件的状态，响应式
- 多个组件需要共享状态时，不同组件要修改同一个状态
- 单一状态树

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
const store = new Vuex.Store({
    state: {
        count: 0
    },
    mutations: {
        increment(state, payload) {
            state.count += payload.count
        }
    },
    actions: {
        increment (context) {
          context.commit('increment')
        }
   }
})
export default store

// 根组件中
import store from 'store.js'
new Vue({
  el: '#app',
  store
})
// 组件中访问
this.$store
```

# State

- 组件中获取状态

  ```javascript
  this.$store.state.xxx  // 一般在计算属性中return
  // 获取多个状态时使用: mapState函数，返回一个对象
  import { mapState } from 'vuex'
  
  computed() {
      // 映射
      ...mapState({
          count: state => state.count,
          cnt: 'count',   // 'count' <=> state => state.count
      })
      // 名字相同时可以传一个数组
      ...mapState([
          'count'  // 相当于 this.count = store.state.count
      ])
  }
  ```

# Getters

- store 的计算属性，state 作为第一个参数，可以接收其他 getter 作为第二个参数

- 会暴露为 store.getters 对象，以属性的方式访问

- 通过方法访问，getter 返回一个接收参数的函数

- 映射到组件的计算属性：

  ```javascript
  import { mapGetters } from 'vuex'
  
  ...mapGetters(['xxx', 'yyy'])
  // 重命名
  ...mapGetters({
      xxx: 'yyy'
  })
  ```

# Mutations

- 更改 store 中的状态的唯一方法是提交 mutation，类似于事件，必须是同步任务

  > comitting变量为true时，才能修改state

- 每个 mutation 都有一个事件类型和回调函数，在回调函数中更改状态，第一个参数是 state

  ```javascript
  store.commit('increment', 7)
  // 传入的参数可以是一个对象
  store.commit('increment', {count: 7})
  
  // 另一种提交方式
  store.commit({
    type: 'increment',
    count: 10
  })
  ```

- 映射：`...mapMutations`

# Actions

- 在 action 中提交 mutation，可以执行异步操作

- 第一个参数接收一个和 store 实例具有相同属性和方法的 `context` 对象，用`context.commit`提交

  ```javascript
  // 可以使用参数解构
  increment ({ commit }) {
      commit('increment')
  }
  ```

- 触发 Action

  ```javascript
  store.dispatch('increment')  // 触发方式和mutation的提交方式相同
  ```

- 组件中触发：`this.$store.dispatch()`或者`...mapActions`映射

- dispatch 返回的是一个Promise

# Modules

- 项目过大时可以分割模块，每个模块拥有自己的属性和方法

  ```javascript
  const moduleA = {
    state: () => ({ ... }),
    mutations: { ... },
    actions: { ... },
    getters: { ... }
  }
  const moduleB = {
    state: () => ({ ... }),
    mutations: { ... },
    actions: { ... }
  }
  const store = new Vuex.Store({
    modules: {
      moduleA,
      moduleB
    }
  })
  ```

- 局部模块中的 action，根节点状态 `context.rootState`

- 局部模块的 getter 的第三个参数是根节点状态：`rootState`

- 带命名空间的模块：

  ```javascript
  account: {
      namespaced: true,
      ...
  }
  // 访问全局内容
  // getter 的第四个参数是rootGetters
  // 全局的分发和提交传入 {root:true}
  dispatch('someOtherAction', null, { root: true })
  commit('someMutation', null, { root: true })
  
  // 注册全局action
  // 添加 `root:true`, action 定义放在 handler 函数中
  someAction: {
      root: true,
      handler (namespacedContext, payload) { ... } 
  }
  ```

# 使用场景

- 音乐播放，购物车，登录状态