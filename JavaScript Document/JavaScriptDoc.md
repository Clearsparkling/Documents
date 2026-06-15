# 	JavaScript

### Closure(闭包)

#### 闭包

##### 原始定义

在一个**函数环境**中，闭包 = **函数** + **词法环境**

``` javascript
function fn(){
  // 函数外部的词法环境
  let x = 1;
  // 函数
  function fun(){
    
  }
}
```

##### JavaScript中优化后的定义

在经过现代浏览器优化后，在原始定义的闭包内，没有使用到词法环境中定义的变量，避免内存问题，就不建立闭包。

``` javascript
function fn() {
	let x = 1
  function fun() {
    return x
  }
  fun()
}
```

闭包是在函数内部创建一个内部函数，**内部函数**能访问它的*外部作用域*

**外部**也可以访问*函数内部的变量*

在JavaScript中闭包会随着函数的创建而创建

##### 闭包的基本格式

``` javascript
function init() {
  var name = "Mozilla"
  funcation displayName() {
    console.log(name)
  }
  return displayNmae
}

const myFunc = init()
myFunc()
```

#### 闭包会导致内存泄漏？

在非极端情况下，闭包并不会导致内存泄漏

通常我们所说的闭包会导致内存泄漏，是因为在对象没有被引用时候，并没有被GC回收这部分内存。

但闭包的生命周期与函数的生命周期是相同的在所属的生命周期结束时，这部分对象也同时会被GC回收

只不过在生命周期内，在对象未被引用时GC不会主动回收该内存空间，但这并非传统意义上的内存泄漏，这更像是开发者编码规范问题导致的内存滥用，在引用结束之后将**引用对象置空**就可以解决这个问题。

**例：**

``` javascript
funcation createFn() {
  const obj = {
    a: 1
  }
  
  return funcation (){
    return obj
  }
}

let fn = createFn()

let b = fn()

funcation onClick() {
  b = null
  fn = null
}
```
