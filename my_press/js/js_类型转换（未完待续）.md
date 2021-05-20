---
title: JS类型转换
category: js
tag:
  - 类型转换
comment: false
---

# JS数据类型及其转换

## 前言
在看书时发现作者在柯里化功能的实现上使用了如下方式：
```javascript
 // add(2)(3)(4)
 const add = (...args)=>{
     const fn = (...args2)=>{
         args = [...args,...args2]
         return fn
     }
     fn.toString = ()=>{
        return args.reduce((total,item)=>total+item,0)
     }
     return fn
 }
```
这里不理解的一点是为什么要声明一个看似与需求无关的toString方法呢，通过学习了解到这里包括一层隐式调用。
> toString表示当该对象被表示为一个文本值时，或者一个对象以预期的字符串方式引用时自动调用。

但是在查看一些关于toString相关的博客时，都会介绍到valueOf和类型转换，但是看了之后感觉云里雾里，所以整理了这篇文章，意在介绍js的类型转换规则。

> 注：这篇文章需要首先了解基础的类型转换规律

## 出现隐式调用的原因
我们都知道javascript是弱类型语言，这也就意味着变量的类型不需要我们提前声明，他会在运行过程中确定，因此就会出现如下情况：
```
1 + '1' // '11'
1 + true // 2
true + '' // 'true'
...
```
这也就是我们熟悉的隐式类型转换，但是请看如下情况：
```javascript
const obj = {
    name: 'zs'
}
1 + obj // '1[Object object]' 
+obj // NaN
```
通过查看ES规范，我们可以知道，在进行隐式转换过程中内部会调用抽象方法toPrimitive

### 1. toPrimitive
该方法接受两个参数，一个是我们当前的操作数input，一个是可选参数PreferredType，PreferredType类型默认为空。

当我们操作的类型是一个Object时它的规则如下：
1. 如果PreferredType没有传，则hint为“default”
2. 如果PreferredType是String，则hint为“string”
3. 如果PreferredType是Number，则hint为“number”
4. 让exoticToPrim为GetMethod(input,@@toPrimitive)，可以理解为获取input参数的@@toPrimitive方法。
5. 如果exoticToPrim不为undefined，也就是说存在@@toPrimitive方法时
    1. 执行exoticToPrim(hint)
    2. 如果result是原始类型则返回result
    3. 否则返回TypeError
6. 如果hint为“default”，则修改hint的值为“number”
7. 执行OridinaryToPrimitive(input,hint)

### 2. OridinaryToPrimitive
该方法主要是做对象类型的转换，同样接受两个参数O（Object）和hint

1. 如果hint为“string”时，则让methodNames为<toString,valueOf>
2. 否则则让methodNames为<valueOf,toString>
3. 遍历methodsName，获取到方法名name
    1. 让method为Get(O,name)，可以理解为获取到指定方法
    2. 判断method是否可调用，如果可以调用则：
        1. 让result为Call(method, O)，理解为调用method方法并赋值给result
        2. 如果result的类型为基本数据类型则直接返回


### 转换流程分析
现在我们已经大概了解了转换规则，接下来我们根据上面的例子分析一下转换规则
```javascript
1 + obj
// 相当于 toPrimitive(1) + toPrimitive(obj)，因为1已经是原始类型所以不需要额外操作。
// 针对toPrimitive(obj)
// 1. 根据规则 1.1此时hint为default
// 2. 根据规则 1.6此时hint为number，并调用OriginaryToPrimitive(input,"number")
// 3. 根据规则 2.2此时methodNames为<valueOf,toString>
// 4. 根据规则 2.3.2.1依次调用valueOf，toString方法得到结果'1[Object object]'
```

> 注： toString方法和valueOf方法是可以重写的，调用规则如以上规则，这里就不过多介绍了

## 总结
在进行类型转换中，如果参与类型转换的参数类型为基本数据类型，则仅仅需要考虑做符号操作时的隐式类型转换，如果存在引用数据类型则按照以上规则进行转换。


## 参考文献
- [ECMAScript® 2015 Language Specification](https://262.ecma-international.org/6.0/)
 