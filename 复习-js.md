# 闭包

> 正常情况下，函数外部是无法读取函数内部声明的变量的

```javascript
function func1() {
  var x = 123
}

console.log(x);  // x is not defined
```

### 概念

- 闭包就是能够读取其他函数内部变量的函数

- 创建闭包最常见的方式：在一个函数的内部定义另一个函数

  ```javascript
  function func1() {
    var x = 123
    function func2() {
      console.log(x)
    }
    func2()
  }
  
  func1()
  // func2 就是闭包
  // 嵌套函数可访问声明于它们外部作用域的变量
  ```

  ```javascript
  function makeAdder(x) {
    return function(y) {
      return x + y
    }
  }
  // add5 和 add10 都是闭包，但是保存了不同的词法环境（所以 x 的值不同
  var add5 = makeAdder(5)
  var add10 = makeAdder(10)
  // 一个函数中的局部变量仅存在于此函数的执行期间
  // 但是形成比闭包后仍旧可以访问
  
  console.log(add5(2))  // 7
  console.log(add10(2)) // 12
  ```

### 用途

- 读取函数内部的变量，让这些变量始终保持在内存中，不会被回收

- 方便调用上下文的局部变量，利于代码封装
- 设计私有方法和变量，避免全局变量的污染

### 缺点

- 内存消耗大，滥用闭包会影响网页性能，可能会导致内存泄漏（退出函数前将不适用的局部变量删除

  > 内存泄漏：申请的内存空间没有被正确释放，导致后续程序里这块内存被永远占用，导致内存浪费

- 闭包会在函数外部改变函数内部变量的值

# new 命令原理

- 执行步骤：
  - 创建一个空对象（要返回的实例对象
  - 把这个空对象的原型指向构造函数的 `prototype` 对象
  - 将 `this` 指向这个空对象：`Object.call(newObj)`
  - 执行构造函数内部的代码

- 构造函数内部的 this 指的是一个新的空对象，构造函数的目的就是把一个空对象构造成需要的样子
- 如果构造函数有 return语句且后面是一个对象，new命令会返回这个对象（不是对象会被忽略
- 没有return语句会返回this对象
- 普通函数使用new，返回一个空对象
- 总结：new命令只会返回一个对象（this对象 or return后的对象

# 构造函数

```javascript
var Vehicle = function () {
  this.price = 1000
}
var Vehicle2 = function (p) {
  this.price = p
}
// Vehicle 就是一个构造函数（第一个字母通常大写
// 函数内部使用 this ，指向对象的实例
// 生成对象时要使用 new 命令
var v = new Vehicle()
v.price // 1000

// 构造函数也可以有参数
var v = new Vehicle2(500)
v.price // 500

// 如果没有使用 new，构造函数就变成了普通函数，this 会指向全局对象
// 严格模式下没有哦使用 new，直接调用会报错
// 也可以在构造函数内部判断是否使用 new（this是否为构造函数Fubar的实例），没有使用则直接返回一个实例对象
function Fubar(foo, bar) {
  if (!(this instanceof Fubar)) {
    return new Fubar(foo, bar);
  }
  ...
}
```

# instanceof 运算符

- 返回一个布尔值，表示该对象是否为某个构造函数的实例

- 左边是实例对象，右边是构造函数

- 原理：检查右边构造函数的`prototype`属性，是否在左边对象的原型链上

- 只能用于对象，不能用于原始类型值

  ```javascript
  var v = new Vehicle();
  v instanceof Vehicle // true
  // 等于
  Vehicle.prototype.isPrototypeOf(v)
  
  // 检查整个原型链
  v instanceof Object // true
  
  // 除了null都是Object的实例
  null instanceof Object // false
  undefined instanceof Object // false
  
  // 如果左边对象的原型链上，只有null对象，instanceof判断会失真（唯一情况
  var obj = Object.create(null);
  typeof obj // "object"
  obj instanceof Object // false
  ```

# Object 对象方法

### isPrototypeOf()

- 判断该实例对象是否在参数对象的原型链上

  ```javascript
  var o1 = {};
  var o2 = Object.create(o1);
  var o3 = Object.create(o2);
  // o1、o2 都是 o3 的原型
  o2.isPrototypeOf(o3) // true
  o1.isPrototypeOf(o3) // true
  ```

- `Object.prototype`处于原型链的最顶端，对各种实例都返回`true`，除了直接继承自`null`的对象（`Object.create(null)`

# 宏任务和微任务

- 宏任务：script代码，setTimeout，setInternal，I/O，setImmediate，requestAnimationFrame（UI render
- 微任务：Promise，MutationObserver，process.nextTick

运行机制：

- 执行一个宏任务
- 执行过程中遇到微任务则添加到微任务列表 
- 宏任务执行完成，指向微任务队列中的所有微任务
- 渲染，开始下一个宏任务

# 防抖和节流

- 防抖：触发事件后，n秒内函数只执行一次，如果n秒内又触发了该事件，就重新计时，n秒内没有再触发则执行函数（连续操作结束后再执行
  - 搜索联想，resize事件
- 节流：连续触发事件时，n 秒内只执行一次函数，减少函数的执行频率
  - 鼠标点击事件，页面滚动事件

# 数组常用方法（*

### 改变原数组的方法

1. splice()：删除数组的一部分，可以在删除位置添加新元素，返回值为被删除的元素

   - 第一个参数是开始位置（从0开始），第二个参数是要删除的个数，再之后的参数表示要添加的新元素
   - 开始位置为负数表示倒数位置
   - 只插入元素可以设置删除个数为0
   - 只有第一个参数，在指定位置将原数组拆成两个数组

2. sort()：默认按照字典顺序排序

   ```javascript
   // 自定义方式排序
   // 回调函数的返回值大于0，交换位置；其他情况都不交换
   // 升序，降序 b-a
   [10111, 1101, 111].sort(function (a, b) {
     return a - b;
   })
   // [111, 1101, 10111]
   ```

3. pop()：删除数组的最后一个元素，返回值为被删除的元素

   ```javascript
   // 空数组使用返回 undefined
   [].pop()  // undefined
   ```

4. shift()：删除数组的第一个元素，返回值为被删除的元素

5. push()：在数组末端添加一个或多个元素（逗号隔开），返回值为新数组的长度

6. unshift()：在数组开头添加一个或多个元素，返回值为新数组的长度

7. reverse()：翻转数组，返回值为改变后的数组

8. copyWith()

9. fill()

### 不改变原数组的方法

1. slice()：提取数组的一部分，返回一个新数组

   - 第一个参数为开始的位置，第二个参数为结束位置（不包括）
   - 省略第二个参数返回到最后
   - 没有参数返回原数组的拷贝
   - 参数为负表示倒数位置
   - 第一个参数大于数组长度，第二个参数小于第一个参数都返回空数组[]

   ```javascript
   // 应用：将类数组转换成真数组
   Array.prototype.slice.call({ 0: 'a', 1: 'b', length: 2 })  // ['a', 'b']
   Array.prototype.slice.call(document.querySelectorAll('div'))
   Array.prototype.slice.call(arguments)
   ```

2. join()：以指定参数作为分隔符，将所有数组成员连接为一个字符串并返回，参数默认值为逗号

   ```javascript
   // undefined、null、空位会被转成空字符串
   [undefined, null].join('#')  // '#'
   ['a',, 'b'].join('-')  // 'a--b'
   
   // 通过 call 方法可用于字符串或类数组
   Array.prototype.join.call('hello', '-')  // "h-e-l-l-o"
   var obj = { 0: 'a', 1: 'b', length: 2 };
   Array.prototype.join.call(obj, '-')  // 'a-b'
   ```

3. concat()：合并数组，参数数组添加到尾部，返回一个新数组

   ```javascript
   // 参数可以使其他类型
   [1, 2, 3].concat(4, 5, 6)
   // 如果原数组成员包含对象，新数组为浅拷贝（对象的引用
   // 原对象改变，新数组也会改变
   ```

4. indexOf(), lastIndexOf()

   - 元素在数组中第一次/最后一次出现的位置，没有出现返回-1
   - 第二个参数表示开始搜索的位置
   - 不能用于NaN，因为函数内部使用严格相等

5. includes()

# 原型和原型链（*

- 所有对象都有自己的原型对象（prototype，任何一个对象都可以是其他对象的原型，原型对象也有自己的原型，就会形成一个原型链

# 绑定事件的方式

- html的on-属性

  ```html
  <button onclick="func()"></button>
  <!-- 只会在冒泡阶段触发，执行函数要加小括号
  	=== el.setAttribute('onclick', 'func()')
  -->
  ```

- 事件属性

  ```javascript
  window.onload = func  // 函数名不用加小括号
  btn.onclick = function() {}
  // 只会在冒泡阶段触发
  ```

- addEventListener

  ```javascript
  btn.addEventListener('click', function() {})
  // 第三个参数表示在什么阶段触发，默认值为false，只在冒泡阶段触发
  // 当前对象的同一个事件，可以添加多个不同的监听函数，添加同一个监听函数只会触发一次
  // 函数内部的 this 指向添加该事件的对象
  ```

# Event对象（*

### 概念

- 事件发生后，会产生一个事件对象传给监听函数，所有的对象都继承了`Event.prototype`实例
- `Event`对象本身也是一个构造函数，可以生成新的实例

### 实例属性

- `Event.bubbles`：返回一个布尔值，表示当前事件是否会冒泡（只读

  > `Event`构造函数生成的事件，默认是不冒泡的，除非显示指定bubbles为true

- `Event.eventPhase`：返回一个整数，表示事件当前所处的阶段（只读

  > 0，事件还没有发生
  >
  > 1，捕获阶段，祖先节点到目标节点
  >
  > 2，到达目标节点（触发事件的元素
  >
  > 3，冒泡阶段，目标节点到祖先的反向传播

- `Event.cancelable`：返回一个布尔值，表示事件是否可以取消（只读

  > `Event`构造函数生成的事件，默认是不可以取消的，除非显示指定cancelable为true

- `Event.cancelBubble`：返回一个布尔值，相当于执行`Event.stopPropagation()`，阻止事件传播
- `Event.target`：触发事件的元素
- `Event.currentTarget`：当前执行的监听函数所在元素（会改变
- `Event.type`：表示事件类型，生成时指定（只读
- `Event.timeStamp`：返回一个毫秒时间戳，表示事件发生的时间，网页加载成功时开始计算
- `Event.isTrusted`：返回一个布尔值，表示该事件是否由真实的用户行为产生（点击事件
- `Event.detail`

### 实例方法

- `Event.preventDefault()`：取消浏览器对当前事件的默认行为（cancelable为true调用才有效
- `Event.stopPropagation()`：阻止事件在 DOM 中继续传播（冒泡，防止触发定义在别的节点上的监听函数，不包括当前节点的其他监听函数
- `Event.stopImmediatePropagation()`：阻止同一个事件的其他监听函数被调用，不管监听函数定义在当前节点还是其他节点（比上面的方法阻止的更彻底
- `Event.composedPath()`：返回一个数组，成员是事件的最底层节点冒泡到最上层的所有节点

# 深拷贝和浅拷贝

- 浅拷贝：只会复制指针，不复制对象本身，共享同一个内存地址，只复制第一层属性
- 深拷贝：创造一个一样的对象，不共享内存，修改新对象不会影响原对象，对属性进行递归复制

# DOM操作

- 创建节点

  ```javascript
  // DocumentFragment 节点代表一个文档的片段，本身就是一个完整的 DOM 树形结构，没有父节点
  // 用于构建一个 DOM 结构，然后插入当前文档，但是它本身不能被插入当前文档，是它的所有子节点插入当前文档
  // 一旦 DocumentFragment 节点被添加进当前文档，它自身就变成了空节点，可以被再次使用
  // 使用 cloneNode 可以不被清空
  document.querySelector('ul').appendChild(docFrag.cloneNode(true));
  
  // 创建 DocumentFragment 节点
  var docFrag = document.createDocumentFragment()
  // 等同于
  var docFrag = new DocumentFragment()
  
  // 创建元素节点并返回，参数为标签名（大小写不敏感，可以是自定义的标签名
  var newDiv = document.createElement('div')
  
  // 创建文本节点并返回，参数是文本内容
  createTextNode()
  ```

- 添加、移除、替换、插入

  ```javascript
  // 参数为节点对象，插入到当前节点最后并返回这个子节点
  // 如果参数是已经存在的节点，会移动到新位置
  appendChild()
  
  // 参数为要移除的子节点并返回，在父节点上调用
  // 被移除的节点还存在内存中，仍然可以使用，只是不再是dom的一部分
  var divA = document.getElementById('A')
  divA.parentNode.removeChild(divA)
  
  // 将一个新的节点，替换当前节点的某一个子节点
  // 参数一是新节点，参数二是要被替换的子节点
  replaceChild()
  
  // 将某个节点插入到指定位置并返回
  // 参数一是要插入的节点，第二个参数是要插入的位置（子节点的前面
  // 参数二为null，将插到最后
  // 参数一为已存在的节点会移动到新位置
  insertBefore()
  ```

- 查找

  ```javascript
  // 参数是id属性，返回指定id属性的元素节点，没有返回null，大小写敏感
  // 只能在document对象使用
  document.getElementById()
  
  // 选择拥有name属性的 HTML 元素，返回一个类数组（NodeList 实例
  getElementsByName()
  
  // 搜索 HTML 标签名，返回值是一个类数组（HTMLCollection 实例
  document.getElementsByTagName('*')  // 返回所有html元素
  
  // 返回具有指定class的元素节点，大小写敏感，返回值同上（动态集合，改变后会立刻反应到实例
  getElementsByClassName()
  
  // 参数为css选择器，多个选择器用逗号隔开，返回第一个匹配的元素，没有返回null
  // 不能选择伪元素
  querySelector('div, p')
  // 返回所有匹配的元素，NodeList 实例，不是动态集合
  // 如果选择器里面有伪元素的选择器，返回一个空的实例
  querySelectorAll()
  ```

# Ajax 优缺点（*

# 立即执行函数(IIFE)

- JavaScript 规定，如果`function`关键字出现在行首，一律解释成语句

  ```javascript
  (function(){ /* code */ }());
  // 或者
  (function(){ /* code */ })();
  ```

- 对匿名函数使用：

  - 不用命名，避免污染全局变量和js库冲突
  - 形成单独的作用域，可以封装外部不能读取的私有变量，避免闭包造成引用变量无发释放

# 多页面之间的通信（*

### cookie

### web worker

### window.sessionStorage & window.localStorage

# CSS和JS动画差异

- 代码复杂度：js更复杂
- 动画控制：js可以暂停、取消、终止，css不能添加事件
- 性能：js要解析

# 同源策略

- 协议相同，域名相同，端口相同（浏览器不遵守，不同端口可以互相读取cookie

- 非同源的限制：
  - 无法读取非同源网页的 Cookie、LocalStorage 和 IndexedDB
  - 无法接触非同源网页的 DOM
  - 无法向非同源地址发送 AJAX 请求（可以发送，但浏览器会拒绝接受响应

#  跨域资源共享(CORS)*

# JS数据类型

- 原始数据类型：Number、String、Boolean、undefine、null、Symbol（ES6增加、BigInt（ES10

  > 占据空间小、大小固定，频繁使用

- 引用类型：Object（包括数组、函数、各种对象

  > 占据空间大、大小不固定，存储的是指向地址的指针

# 数据类型转换*

### 强制转换

- `Number()`：将任意类型的值转换为数值
- `String()`：将任意类型的值转换为字符串
- `Boolean()`：将任意类型的值转换为布尔值

### 自动转换

- 

# 布尔运算符

- &&：逻辑与，短路运算
- ||：逻辑或，短路运算
- !：取反运算
- `?:`：三元运算符
