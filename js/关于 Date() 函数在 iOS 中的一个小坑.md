# 关于 Date() 函数在 iOS 中的一个小坑

## bug

今天遇到了一个诡异的 bug 。一个 `Vux` 的日期选择组件在 PC 端和安卓端都能正常显示和使用，而在 `iOS` 端却不能正常出现。经过漫长的调试，终于发现问题出在这一行代码上：

```
var startDate = new Date('2017-5-3')
```

这行代码在 PC 端和安卓端都是正常的，而在 `iOS` 端则会提示 `Invalid Date` 无效日期。

## 原因

`new Date(dateString)` 实际上是调用了 `Date.parse()` 这个函数。关于这个函数， `ECMAScript` 规范规定：

>如果一个字符串不符合标准格式，则函数可以使用任何由引擎决定的策略或解析算法。 `Date.parse()` 对于因包含有无效元素而无法识别的 `ISO` 格式字符串或者日期应该返回 `NaN` 。

简单的说这个函数在不同的浏览器引擎中会存在偏差，导致对字符串的解析不一致或部分浏览器无法解析。问题应该就是出在这里！是 `Safari` 不能识别这串字符串。

经过测试，将代码改为如下样子：

```
var startDate = new Date('2017-05-03')
```

这样代码就可以 `iOS` 端正常运行了。坑爹的 `Safari` 。

## 参考链接
[Date.parse - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/parse)