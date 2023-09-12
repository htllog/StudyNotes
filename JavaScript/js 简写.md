# 关于插件

 [My top JavaScript utilities, in just One Line of Code](https://phuoc.ng/collection/1-loc/)

一行代码实现一个方法的网站

# if - Else 用 || 或 ?? 运算符简化

* [逻辑或操作符](https://github.com/htllog/StudyNotes/blob/main/JavaScript/%E9%80%BB%E8%BE%91%E6%88%96.md) 需要注意 **0** 和 **' '** 也会被认为是 `false`
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

输入框非空的判断（如不想把 `0` 当成 `false` 使用时）

```js
if (value !== null && value != undefind && value) {}

// 等价写法
if ((value ?? '') !== '') {}
```

# includes 的正确使用姿势

[includes()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes) 实例的方法确定 Array 数组的条目中是否包含某个值，并根据需要 返回 `true` 或 `false`

```js
function Screen(obj) {
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

使用[可选链运算符 ?. ](https://github.com/htllog/StudyNotes/blob/main/JavaScript/%E5%8F%AF%E9%80%89%E9%93%BE%E8%BF%90%E7%AE%97%E7%AC%A6.md)对代码进行简写：

```javascript
const student = {
  name: "htw",
  address: {
    state: "New York",
  },
};

// 一层层嵌套判断
console.log(student && student.a && student.address.ZIPCode); // undefind

// 使用可选链操作符 等价
console.log(student?.address?.ZIPCode);
```

可选链运算符也可以运用于方法调用。如果方法存在，它将被调用，否则返回 `undefined`

```javascript
const obj = {
  foo () {
    console.log('Hello from foo!')
  }
}

obj.foo?.() // 输出 ‘Hello from foo!’
obj.bar?.() // undefined
```

同理可以在数组上使用

```javascript
const arr = [1, 2, 3, 4, 5]

console.log(arr[0]) // 1
console.log(arr[5]) // undefined

// 使用可选链运算符
console.log(arr?.[0]) // 1
console.log(arr?.[5]) // undefined
console.log(arr?.[0]?.toString()) // 1
```


# 快速生成数组

> 利用数组下标值，生成 0 - 9

```javascript
// 方法一：
const arr1 = [...new Array(10).keys()] // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

// 方法二：
const arr2 = Array.from(Array(10), (v, k) => k) // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

> 通过 map 生成 1 - 10

```javascript
const arr3 = [...Array(10)].map((v, i) => i + 1) // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

> 快速生成 10 个 0 的数组

```javascript
const arrs = new Array(10).fill(0) // [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
```

> 快速生成 10 个 [] 的数组（二维数组）

```javascript
// 错误写法
const arrays = new Array(10).fill([]) 

// 二维数组不能直接写成 new Array(10).fill([]), 也就是 fill 方法不能传引用类型的值，[] 换成 new Array() 也不行. 因为 fill 里传入引用类型值会导致每一个数组都指向同一个地址，改变一个数据的时候其他数据也会随之改变

// 正确写法
const arrays = new Array(10).fill().map(() => new Array());

// [Array(0), Array(0), Array(0), Array(0), Array(0), Array(0), Array(0), Array(0), Array(0), Array(0)]
```



