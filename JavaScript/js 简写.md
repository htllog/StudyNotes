# 关于插件

 [My top JavaScript utilities, in just One Line of Code](https://phuoc.ng/collection/1-loc/)

一行代码实现一个方法的网站

# if - Else 用 || 或 ?? 运算符简化

* [逻辑或操作符](https://github.com/htllog/StudyNotes/blob/main/JavaScript/%E9%80%BB%E8%BE%91%E6%88%96.md) 需要注意 **0** 和 **' '** 也会被认为是 false
* 如果[逻辑或操作符](https://github.com/htllog/StudyNotes/blob/main/JavaScript/%E9%80%BB%E8%BE%91%E6%88%96.md) 前面的值为 **0** **' '** **false** **null** **undefined** **NaN** 其中的任意一种，直接返回 [||](https://github.com/htllog/StudyNotes/blob/main/JavaScript/%E9%80%BB%E8%BE%91%E6%88%96.md) 后面的值
* [空值合并操作符 ?? ](https://github.com/htllog/StudyNotes/blob/main/JavaScript/%E7%A9%BA%E5%80%BC%E5%90%88%E5%B9%B6%E8%BF%90%E7%AE%97%E7%AC%A6.md) 如果没有定义左侧返回右侧。如果是，返回左侧



```js
function processObject(obj) {
  var a = obj || {}; // 使用 || 运算符

  var b = obj ?? {}; // 使用 ?? 操作符

  console.log("a:", a, "b:", b);
}

// 示例用法
var exampleObj = null;

processObject(exampleObj); // 'a'和'b' 都将被分配为空对象

// 等价写法
function processObject(obj) {
  var a;

  if (obj === null || obj === undefined) {
    a = {};
  } else if (typeof obj === "number" && isNaN(obj)) {
    a = {};
  } else if (obj === 0 || obj === "" || obj === false) {
    a = {};
  } else {
    a = obj;
  }
}

// 示例用法
var exampleObj = null;

processObject(exampleObj);
```

# 输入框非空的判断

输入框非空的判断（如不想把 0 当成 false 使用时）

```js
if (value !== null && value != undefind && value) {}

// 等价写法
if ((value ?? '') !== '') {}
```

# includes 的正确使用姿势

[includes()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes) 实例的方法确定 Array 数组的条目中是否包含某个值，并根据需要 返回 true 或 false

```js
function Screen() {
  var a;

  if (
    obj === 0 ||
    obj === "" ||
    obj === false ||
    obj === null ||
    obj === undefined ||
    isNaN(obj)
  ) {
    a = {};
  }
}

// 写法等价
function Screen(obj) {
  if ([0, "", false, null, undefined].includes(obj) || isNaN(obj)) {
    var a = {};
  }
}
```

# 防止崩溃的可选链 ?.

使用可选链运算符对代码进行简写：

```
```

