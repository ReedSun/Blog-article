# ES6 中 Class 学习笔记

ES6 中的 `class` （类）实际上就是基于原型继承的语法糖，可以让我们用更简单更清晰的语法来创建类。

这篇文章即为我学习 `class` 的学习笔记，其中内容大部分参考 [类 - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes) 。

## 定义类

定义类有两种方式，类表达式和类声明。这一点跟函数很类似，函数使用 `function` 关键词来声明函数，而类则使用 `class` 关键词来声明类。

### 类声明

如下所示即为类声明：

```
class People {
  // xxxx
}
```

建议: 类名首字母最好大写。

注意：类声明不会声明前置，这点与函数不同，要注意。

### 类表达式

类表达式可以命名也可以是匿名的。如下所示：

```
// 匿名
let People = class {
  // xxx
}

// 命名
let People = class People {
  // xxx
}
```

注意：类表达式同样会收到不声明前置的影响

## 类体和类方法

一个类的类体是又一对中括号 `{}` 组成的，在其中我们将定义方法或构造函数。这个很像一个对象，但与对象不同的是，类体内不需要逗号分隔不同的方法。

###　构造函数

构造函数( `constructor` )用于：创建或初始化一个**使用这个类创建的实例**。概念似乎很拗口，但是一看例子就知道其含义了。

```
class People {
  constructor (name, age) {
    this.name = name
    this.age = age
  }
}

let me = new People('ReedSun', 23)
console.log(me.name)  // 'ReedSun'
console.log(me.age)  // 23
```

有了例子，我们就能看明白了，构造函数 `constructor` 接收的参数为我们实例化这个类时传入的参数。

注意：一个类方法中只能有一个构造函数，如果有多个构造函数则会抛出异常。

同时，一个构造函数可以使用 `super` 关键词来调用一个父类构造函数。

### 原型方法

原型方法直接写在类体中就可以

```
class People {
  constructor (name) {
    this.name = name
  }
  getName () {
    console.log('Name: ' + this.name)
  }
}

let me = new People('ReedSun')
me.getName()  // name: ReedSun
```

### 静态方法

静态方法需要使用 `static` 来声明，如下所示：

```
class People {
  constructor (name) {
    this.name = name
  }
  static walk (num) {
    console.log('我走了' + num + '步')
  }
}

People.walk(100)  // 我走了100步
```

**静态方法是类本身的方法，原型方法是类的实例的方法。**你懂了嘛？我反正懂了~

## 创建子类

为一个类创建子类需要使用 `extends` 关键字，如下所示：

```
class People {
  constructor (name) {
    this.name
  }
  getHeight () {
    console.log(this.name + ' is people.')
  }
  getWord () {
    console.log(this.name + ' is saying.')
  }
}

class Teen extends People {
  constructor (name) {
    super()
    this.name = name
  }
  getHeight () {
    console.log(this.name + ' is teen.')
  }
}

let me = new Teen('ReedSun')
me.getHeight()  // ReedSun is teen.
me.getWord()  // ReedSun is saying.
```

注意：如果子类中存在构造函数，则需要在使用 `this` 之前首先调用 `super()`。

## 参考文章

[类 - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes)