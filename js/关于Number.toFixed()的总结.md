# 关于Number.toFixed()函数的总结

## 前言

今天工作中遇到了一个需求，需要将类似于 `1.99999` 这样的数字格式化为 `2.00` 这样的两位小数。本来打算自己实现一个类似的功能函数，但是没想到看起来容易，实际实现起来却还是有点复杂的，就例如逢9进位这样的功能就让我想的有点头疼。索性考虑起来大谷歌来实现这样的功能。

没想到谷歌一下发现，居然 JS 中现成的函数 `Number.toFixed()` 来实现我这样的需求。赶紧查阅了[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed)，然后写下这篇文章来总结一下 `Number.toFixed()` 的各个方面。

## 语法

```
numObj.toFixed(digits)
```

### 参数

|参数|含义|
|--|--|
|`digits`|小数点后数字的个数；介于 0 到 20 （包括）之间，实现环境可能支持更大范围。如果忽略该参数，则默认为 0|

### 返回值

该方法不会改变原数值。

该方法会返回一个数值的字符串表现形式，保留 `digits` 位小数。

如果有必要会进行四舍五入。

如果原数字位数不足 `digits` 位则会补零。

如果数值大于 1e+21，该方法会简单调用 `Number.prototype.toString()` 并返回一个指数记数法格式的字符串。

### 异常

|可能抛出的异常|原因|
|--|--|
|`RangeError`|如果 `digits` 参数太小或太大则可能抛出此异常。一般来说 0 到 20（包括）之间的值不会引起 RangeError。实现环境（implementations）也可以支持更大或更小的值|
|`TypeErrpr`|如果该方法在一个非Number类型的对象上调用将会抛出此异常|

## 真实需求
在实际情况中我们可能会面对这样的需求：如果数字小数点后位数大于两位则格式化到两位，如果不大于两位则不格式化。即不需要 `Number.toFixed()` 函数的补零功能。我们可以这样实现：

```
function () {
  let valueStr = value.toString()
  if (valueStr.indexOf('.') !== -1 && valueStr.split('.')[1].length > 2) {
    return value.toFixed(2)
  } else {
    return value
  }  
}
```

在这个函数中，我们通过判断 `'.'` 是否存在，以及 `'.'` 之后数字的个数来判断我们是否需要格式化，这样就完美解决了我们的需求。

## 参考资料

[Number.prototype.toFixed() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed)
