# 实例方法

#### map()

- 遍历数组，把每一次的执行结果组成一个新数组返回
- 回调函数接收三个参数：item，index，arr
- 如果数组有空位会跳过，不会执行回调，不会跳过 `undefined` 和 `null`

#### farEach()

- 遍历数组，没有返回值
- 回调函数接收三个参数：item，index，arr
- 无法中断，会把所有成员遍历完，中断遍历要使用 for 循环
- 如果数组有空位会跳过，不会执行回调，不会跳过 `undefined` 和 `null`

#### filter() 

- 遍历数组，返回值为 true 的成员组成一个新数组返回，不会改变原数组
- 回调函数接收三个参数：item，index，arr

#### some() & every()

- 返回一个 Boolean，判断数组成员是否符合某种条件
- 接收一个函数为参数，这个函数接收三个参数：item，index，arr
- `some()` ：只要有一个成员返回 `true`，整个方法就返回 `true`
- `every()`：所有成员返回 `true`，整个方法才返回 `true`
- 如果是空数组，`some` 返回 `false`，`every` 返回 `true`，回调函数不会执行

#### reduce() & reducecRight()

- 依次处理数组的每个成员，最终累计为一个值，`reduceRight` 是从右向左处理

