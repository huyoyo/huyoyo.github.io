---
layout: post
title: "记录一道面试题－柯里化(currying)"
date: 2017-09-04 19:28:13 +0800
comments: true
categories: js面试 读书笔记
tags: 
---
&nbsp;&nbsp;大美团今日一面，首先是一道简答题，问一个add(x)函数可以打印出x的值，问add(1,2)会输出什么，如果是输出add(1)(2)呢？如何实现 add(1)(2)输出为3? 如果持续增加后续呢？（**递归的一个函数列表调用**），那么我们一个个解决吧～～<!--more-->
####考点
1.arguments参数数组的运用

2.函数式编程的了解

3.js数据类型的转换

4.console在输出的时候会自动调用什么函数

5.对闭包和链式调用的理解

6.自身想法，柯里化到底有什么用啊啊啊？！！！

####初级－打印输出结果
```javascript
function add1(){
  var args = [].slice.call(arguments); //重新拷贝一份
  console.log(args)} 
}

function add(x){
    console.log(arguments);
    console.log(x);
  }
  add1(1,2);//(2)[1,2]->{0:1,1:2,length:2,_proto_:Array[0]}
  add(1,2);//(2) [1, 2, callee: ƒ, Symbol(Symbol.iterator): ƒ，__proto__
:Object];1
  add(1)(2);//1;Uncaught TypeError: add(...) is not a function

```
&nbsp;&nbsp;我们可以看到调用函数设置不同参数的结果，**add(1,2)**实际考察的是**arguments**,它是一个类数组对象，它的属性名是按照传入参数的序列来的，第1个参数的属性名是’0’，第2个参数的属性名是’1’，以此类推，并且它还有`length`属性，存储的是当前传入函数参数的个数(实参)。从**add(1,2)**输出的打印**arguments**可以看到是由`Object`构造出来的。**add1(1,2)**和**add(1,2)**的区别在于，我们在一开始调用call函数进行拷贝（这里有深拷贝浅拷贝的坑，就不详细展开啦～），利用数组方法slice拷贝一份出来后，这个打印的结果`_proto_`就指向`Array`了，因为我们`call`了数组方法～。虽然失去一些特性和函数，但是基本数据还是在的，比如length，以及参数。

&nbsp;&nbsp;另外补充一点，`callee`是**arguments**对象中有一个非常有用的属性。`arguments.callee`返回此**arguments**对象所在的当前函数引用。在使用函数递归调用时推荐使用arguments.callee代替函数名本身。

```javascript
function count(a){
    if(a==1){
        return 1;
    } 
    return a + arguments.callee(--a);
}

var mm = count(10);
alert(mm);//55;(1+2+3+4+...10)
```
####中级－－实现add(1)(2)

&nbsp;&nbsp;直观的对这么个函数，可以认为是先调用add(1)得到一个返回结果－－返回结果是个函数啊－－继续调用一个；就是最后一个（）去返回你叠加的值，面试时候就是这么粗暴的答题，结果引起后续拓展的challenge了，哭哭TT

&nbsp;&nbsp;不管怎么样，先来一段粗暴的代码freestyle~

```javascript
function add(x){
  return function(y){
   return x+y;
  }
}
add(1)(2)//1+2=3;
```
&nbsp;&nbsp;结果表明可以达到预期的输出，但是如果是实现一个参数个数任意的相加处理函数呢，楼上这个枚举肯定不行啊少年！那么一层一层，最后返回。这个不就是递归吗？？？我面试想到递归后，觉得那么出口在哪里？什么时候去return你一开始的这个参数啊。回来想明白了，当你参数是()时候，自然就表明这个递归结束了，这个就是出口啊！！！那么结合**arguments**，判断条件就是`length=0`,继续写一段代码：

```javascript
 function add(x){
   sum=0;
   sum=sum+x;
  return function tempfunction(y){
    console.log(arguments);
    if(arguments.length===0)
    {
      return sum;
    }
    else{
     sum=sum+y;
     return tempfunction;
    
    }
    
  } 
  
 }
 console.log(add(1)(2));//有坑 输出都是函数字符串
 console.log(add(1)(2)())//3;
```
&nbsp;&nbsp;debug好久啊，并不知道为什么一开始。原来是add(1)(2)这样你的`length`一直是1啊！！所有加一个()作为返回结果的最终条件。我们还可以换一个写法，重写valueOf,toString。**原理**：其实原理都是使用闭包记住了`temp`中`x`的值，第一次调用add(),初始化了`temp`，并将`x`保存在`temp`的作用链中，然后返回`temp`保证了第二次调用的是`temp`函数，后面的计算都是在调用`temp`, 因为`temp`也是返回的自己，保证了第二次之后的调用也是调用`temp`，而在`temp`中将传入的参数与保存在作用链中x相加并付给sum，这样就保证了计算；但是在计算完成后还是返回了tmp这个函数，这样就获取不到计算的结果了，我们需要的结果是一个计算的数字那么怎么办呢，首先要知道JavaScript中，打印和相加计算，会分别调用toString或valueOf函数，所以我们重写tmp的toString和valueOf方法，返回sum的值；

(**利用JS中对象到原始值的转换规则:当一个对象转换成原始值时，先查看对象是否有valueOf方法，如果有并且返回值是一个原始值，
那么直接返回这个值，否则没有valueOf或返回的不是原始值，那么调用toString方法，返回字符串表示**.)

看代码：

```javascript
 function add(x){
    var sum=0;
    sum=sum+x;
    var temp=function(y){
       if(arguments.length===0){
        return sum;
       }else{
        sum=sum+y;
        return temp;
       }
    }
    
    temp.toString=function(){
     return sum;
    }
    temp.valueOf=function(){
      return sum;
    }
    return temp;
 }
 console.log(add(1)(2)(3));//f 6 typeof看一下是个function
 console.log(+add(1)(2)(3));//Number类型强转化成数字,6
```
&nbsp;&nbsp;我们还可以换一个写法，不用那么多的var的临时变量，简单点，就是递归调用add啊～ 

```javascript
function add(x){
   var temp=function tempfunction(y){  
   return add(x+y);
   }
   temp.toString=function(){
     return x;
   }
   temp.valueOf=function(){
     return x;
   }
   return temp
}
console.log(add(1)(2)(3));// f 6
console.log(+add(1)(2)(3));// 6
console.log(add(1,2,3)(2)(3));//6 并不是会1+2+3+2+3哦

```

####高阶－－柯里化
&nbsp;&nbsp;柯里化这个概念，第一次是在头条面试看到(currying)面试官大佬打了个英文就说，算了。我们开个数组去重的题目吧呵呵呵＝ ＝（ps:数组去重写对了indexOf都不行，还有想到ES6的 Set()特性，头条大佬很任性啊！！！！！）拉回来，导致我并不知道这个高端词汇，

Google的解释如下：

>在计算机科学中，柯里化（英语：Currying），又译为卡瑞化或加里化，是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。
>柯里化其实本身是固定一个可以预期的参数，并返回一个特定的函数，处理批特定的需求。这增加了函数的适用性，但同时也降低了函数的适用范围。

&nbsp;&nbsp;我们返回来看前面那个坑！前面方法不适用于add(1,2,3)(1)(2)这种啊，只会输出结果为`1+1+2=4`因为我们就没有遍历我们的arguments数据的所有值啊，默认就是arguments[0]啊朋友们。
看个通用实现好吗～：

```javascript
function currying(fn) {
     var slice = Array.prototype.slice,
     __args = slice.call(arguments, 1);
     return function () {
            var __inargs = slice.call(arguments);
          return fn.apply(null,  __args.concat(__inargs));
            };
        }
```
&nbsp;&nbsp;这段代码，首先是arguments调用slice方法去拷贝一份给_args，然后在返回的时候把arguments的参数连接起来用`concat`，重新生成一个数组，作为函数的返回值，这样最后得到一个新的参数数组。然后统一调用某一方法。由此我们可以实现如下代码:

```javascript
 function add(){
    var arg=[].slice.call(arguments);
    var temp=function(){
      var inargs=[].slice.call(arguments);
      return add.apply(null,arg.concat(inagrs));
    }
    temp.toString=function(){
     return args.reduce(function(a,b){
     return a+b;
     });
    }
    return temp
 }
 console.log(add(1,2)(3)(4));//f 10
 console.log(+add(1,2)(3)(4));//10
```
&nbsp;&nbsp;感觉这个很清晰很赞啊，那么问题又来了这个在工程里面有什么好处啊？？每个东西都有存在理由吧？？知乎上的这个答题我觉得很有用，理解很透彻！

>工程上的柯里化主要就是为了统一接口，从而实现更高程度的抽象。先从大家熟悉的概念说起。在我们用oop编程的时候，这层抽象通常是用对象模型，通过继承并且使用基类的虚函数实现。通过继承，我们得到了一个统一的接口，于是只要能够适配基类的运算我们就可以相应的用子类来参与。在使用functional范式的时候，实现这层抽象用的就是柯里化。运算不再是适配基类，而是适配特定函数signature。

####引申--参数复用
&nbsp;&nbsp;当多次调用同一个函数，并且传递的参数绝大多数是相同的时候，那么该函数就是一个很好的柯里化候选。例如, 我们经常会用`Function.prototype.bind`方法来解决上述问题。

```javascript
const obj = { name: 'test' };
const foo = function (prefix, suffix) {
    console.log(prefix + this.name + suffix);
}.bind(obj, 'currying-');

foo('-function'); // currying-test-function
```
与`call`/`apply`方法直接执行不同，`bind`方法将第一个参数设置为函数执行的上下文，其他参数依次传递给调用方法（函数的主体本身不执行，可以看成是延迟执行），并动态创建返回一个新的函数。这很符合柯里化的特征。下面来手动实现一下`bind`方法：

```javascript
Function.prototype.bind = function (context, ...args) {
    return (...rest) => this.call(context, ...args, ...rest);
};
```
####总结
&nbsp;&nbsp;一个面试题引申的知识点真的足够深，大美团面试的小姐姐真的很nice。说我还要更加系统的去展开问题，我觉得summary两个小时写的这个资料还远远不够。里面涉及的`apply`、`call`、`bind`的区别在下一篇文章里面展开吧，听说工程也不怎么用柯里化了，因为出来了`lambda`,能嵌入到其他表达式当中的匿名函数（闭包）。

* 第一个重要意义是可以在表达式当中直接定义一个函数，而不需要将定义函数和表达式分开，这样有助于将逻辑用更紧凑的方式表达出来。

* 它的第二个重要意义是引入了闭包。基本上来说常见的支持lambda表达式的语言里，不存在不支持闭包的lambda表达式；从函数式编程的角度来说，支持闭包也是很重要的。闭包是指将当前作用域中的变量通过值或者引用的方式封装到lambda表达式当中，成为表达式的一部分，它使你的lambda表达式从一个普通的函数变成了一个带隐藏参数的函数。
* 它的第三个重要意义（如果有的话）是允许函数作为一个对象来进行传递。

后续`lambda`怎么用，包括和`map`,`filter`,`reduce`一起合作写？知识点还要继续增强，我爱学习，学习使我快乐～我们下次继续实战加总结，再也不让博客长草了TT.

####Link
* [js函数参数列表返回的简单实现](https://segmentfault.com/q/1010000004499011)
* [实现add(1)(2)(3)](https://segmentfault.com/a/1190000008610969)
* [实现类似于add(1)(2)(3)调用方式的方法](http://www.css88.com/archives/5147/comment-page-1)
* [一道面试题引发的对javascript类型转换的思考](http://www.cnblogs.com/coco1s/p/6509141.html)
* [前端开发者进阶之函数柯里化Currying](http://www.cnblogs.com/pigtail/p/3447660.html)
* [柯里化在工程中有什么好处?](https://www.zhihu.com/question/37774367)
* [reduce 方法 (Array)](https://msdn.microsoft.com/library/ff679975.aspx)
* [有趣的反科里化－－(待看)](http://www.cnblogs.com/hustskyking/archive/2013/04/09/uncurrying.html)
* [Lambda 表达式有何用处？如何使用？－－（待看)](https://www.zhihu.com/question/20125256)
