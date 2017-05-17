# 关于 Storge 在 Safari 的无痕浏览模式中的坑

## 前言

今天遇到了一个诡异的bug，一个网页在微信浏览器中是可以正常显示的，而在一些用户的 Safari 中却无法正常渲染。真是日了狗了！检查一下报错，发现这个报错很可疑：

```
QuotaExceededError
The quota has been exceeded.
```

Google了半天，终于发现了问题的原因可能是出在 Safari 的无痕浏览当中。

> Safari在所谓的private mode下不允许使用LocalStorage功能，只有在用户自身开启non-private mode的情况下才可以正常使用LocalStorage。

果断开无痕浏览试一试，果然网页就打不开了 - -！

## 原因

Safari 的无痕浏览模式会这样处理 `Storge` 对象：

- `Storage` 对象仍然存在。

- 但是 `setItem()` 会报异常：QuotaExceededError。

- `getItem()` 和 `removeItem()` 方法会直接忽略。

## 解决办法

既然知道了问题的原因，解决起来自然也就简单许多了，由于这个问题的解决方案与业务息息相关，我这里就提供几个简单的思路。

1. 在 Safari 的无痕模式中不执行 `Storage.setItem()` 的部分，简单粗暴。

2. 使用 cookie 替代 `Storage` 对象。

至于判断是否可用 `Stroage` 对象，我们可以用 `try...catch...` 语句来判断。
