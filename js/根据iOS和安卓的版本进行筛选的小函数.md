# 简单筛选iOS和安卓的版本的函数

## 需求

现如今，移动端大行其道，我们终于不太需要再为了IE6, IE7, IE8...做可怕的兼容了。但是，仍然有一些库或框架不能再低版本的iOS和安卓中正常运行，所以我就写了一个小函数来筛选iOS和安卓的版本

函数的期盼输入为你期望的iOS和安卓版本，输出为一个布尔值，如果当前设备版本大于你输入的版本，则返回 `true` ，否则返回 `false` 。

## 函数

```
function filterVersion (iOS, Android) {
  if (navigator.userAgent.indexOf('Mac OS X') !== -1) {
    var reg = /OS\s[\d\_]*\slike\sMac\sOS\sX/.exec(navigator.userAgent)
    if (reg) {
      var version = parseFloat(reg[0].slice(3, -14).replace(/_/g, '.'))
      return version >= iOS
      return false
    }
  } else if (navigator.userAgent.indexOf('Android') !== -1) {
    var reg = /Android\s[\d\.]*;/.exec(navigator.userAgent)
    if (reg) {
      var version = parseFloat(reg[0].slice(8, -1))
      return version >= Android
    } else {
      return false
    } 
  } else {
    return false
  }
}
```

## 例子

比如一个功能xxx，你你只想在iOS 10.0以上的版本，及安卓 7.0以上的版本上使用，则可以这样写

```
function xxx () {
  if (!filterVersion(10, 7)) {
    return
  }
  // ....
}
```

## 原理

这个判断方法是根据浏览器提供的UA，即 `window.navigator.userAgent` 来做出判断的，这个属性就提供了安卓或iOS的系统版本信息。