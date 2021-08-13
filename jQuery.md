#### 一个 JavaScript 库， JavaScript + Query（查询）

## 入口函数

```javascript
// 原生 js 入口函数会等到文档、外部的js文件、css文件、图片都加载完毕才会执行
// 只能写一个，写多个会覆盖
window.onload = function() { 
    
}

// jquery 入口函数 DOM 结构渲染完后就会执行
// 可以写很多个
$(document).ready(function() {
    
})
// 其他写法
jQuery(document).ready(function() {
    
})

$(function() {
    
})
```

## $ 冲突

```javascript
// 释放 $ 使用权
jQuery.noConflict()

// 自定义访问符号
const jq = jQuery.noConflict()
```

## 核心函数

```javascript
$()
```

- 接收一个函数（入口函数）
- 接收一个字符串（选择器）
- 接收一个 html 代码片段：返回一个 jQuery 对象
- 接收一个 DOM 元素： 返回一个被包装的 jQuery 对象

## 选择器

```javascript
$('li:first')
$('li:odd')      // 奇数
$('li:even')     // 偶数
```

## css() 方法

```javascript
$('p').css({'属性': 'val'})
```

