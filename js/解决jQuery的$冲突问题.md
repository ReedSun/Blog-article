# 解决jQuery的$冲突问题

很多JavaScript的库使用`$`作为变量名，如果我们同时引用两个使用`$`作为变量名，或者引入两个不同版本的jQuery时，就可能会出现`$`冲突问题。

作为解决办法，我们可以使用
```
jQuery.noConflict()
```
来解决这个问题。

## jQuery.noConflict()
`jQuery.noConflict()`的作用是放弃jQuery控制$ 变量。

## 具体应用方法

### 方法一
一种方法是使用`jQuery`来代替`$`

```
<script src="other_lib.js"></script>
<script src="jquery.js"></script>
<script>
    $.noConflict();    // 让jQuery放弃$变量的控制
    jQuery("div")...   // 使用jQuery来代替$
    $("div")...        // 其他库正常使用$
</script>
```

第一种方法有一个问题是我们必须全部使用`jQuery`来代替`$`，如果我们还想用`$`，我们可以参考以下两种方法。

### 方法二

这种方法是使用`jQuery.ready()`形成闭包，在闭包中运行jQuery代码

`jQuery.ready()` => 当DOM准备就绪时，指定一个函数来执行。

```
<script src="other_lib.js"></script>
<script src="jquery.js"></script>
<script>
    $.noConflict();    // 让jQuery放弃$变量的控制
    jQuery(document).ready(funciton($){
        ...            // jQuery代码在这里 
        })   
    ...                // 其他库代码在这里
</script>
```

### 方法三

这种方法是使用立即执行函数，让`$`作为函数的变量，调用`jQuery`作为`$`。

```
<script src="other_lib.js"></script>
<script src="jquery.js"></script>
<script>
    $.noConflict();    // 让jQuery放弃$变量的控制
    (function($){}
        ...            // jQuery代码在这里
    )(jQuery)  
    ...                // 其他库代码在这里
</script>
```