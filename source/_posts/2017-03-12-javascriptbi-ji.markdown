---
layout: post
title: "javaScript学习笔记(二)"
date: 2017-03-12 10:57:09 +0800
comments: true
categories: 读书笔记
tags: 
---
最近心态不好，莫名其妙忙成狗啊。有项目压力，还要找符合自己的论文paper，最重要的是还有实习面试材料要准备。基础不牢靠背书啊啊啊啊，胡宝宝的心里压力“咚”大好嘛。趁着有时间，把看书的笔记摘摘～<!--more-->

####阅读书目

《javaScript学习指南》

####重点摘要
* 函数类型有三种：

**声明式函数** 拥有自己语句的函数，最开始是关键字function。声明型函数只会解析一次，它是静态的，并且只提供一个名称以便访问它；
	
	function add(m,n){
	aler(m+n);
	}
这种方式等同于构造一个function类的实例的方式:

	var add=new Function("m","n","alert(m+n);");
Function类构造方法的最后一个参数为函数体："alert(m+n);"，前面的都是函数的形参，参数必须是字符串形式的："m","n"。

**匿名函数** 使用构造函数创建的函数。每次访问它时都将解析一次，并且没有指定函数名称;(上述为举例说明)

	//创建一个函数，它的参数是一个数据对象和一个函数，
	它将对这个数据对象调用该函数
	function invokeFunction(dataObject,functionTocall)
	{ functionTocall(dataObject);
	}
	var funCall=new Function('x','alert(x)');
	invokeFunction('hello',funCall);

**函数字面量或函数表达式** 在其他语句或表达式中创建的函数。它只会解析一次，它是静态的，可以指定也可以不制定一个特定的函数名称。如果它是已命名的，那么只能在其定义的函数体内访问它

	function outerFun(base){
	   var test1="!";
	   //返回内部函数
	   return function(ext){
	   return base+ext+test1;
	   }
	}
	//调用事例：
	var baseString=outerFun('hello ');
	var newString=baseString('you ');
	alert(newString);
	//输出结果为： hello you !
	
`javaScript闭包`:当一个内部函数是外部应用程序的返回值，并赋值给一个外部变量的时候，**内部函数的作用域将附加到外部函数上**然后再附加到主调应用程序中，这样才能保证**内部函数和外部函数参数和变量的完整性**返回再其他函数中以内部对象形式创建的一个函数字面量，然后将其赋值给主调应用程序中的一个变量，它将引入一个作用域链的概念，它是确保应用程序在本地能够正常工作所需的数据。

* 浏览器兼容性检查通用方法

**编写一段跨浏览器兼容代码，检查浏览器是否支持该元素**

测试一个是否支持HTML5 element,基本前提是：
 	
 	1. 使用document.createElement()动态创建该元素，判断浏览器是否支持它。
 	2. 测试新创建对象的一个已知属性或js方法是否存在，来判断是否浏览器真的支持该元素。
 	3. 对应input元素，设置你要测试的input元素的Type 属性，然后看浏览器是否保留该值。

**一个例子**	
	
	//测试是否在style对象中实现了textShadow
	var headerElement=doucument.getElementById("pageHeader");
	headerElement.style.textShadow="#ff0000 2px 2px 3px";
	
* DOM Level2 事件模型

**支持事件捕获（前远后近）和事件冒泡**两种处理方式
	
	//level0和2指定一个事件的区别
	document.onclick=clickFunc;
	document.addEventListener("click", clickFunc,false);
	
	//考虑代码能安全的运行在所有浏览器上	
	if(document.addEventListener)
	{
      document.addEventListener("click", clickFunc,false);
      }
      else if(document.attachEvent){
    document.attachEvent("onclick", clickFunc); 
    }else if(document.onclick)
    {
     document.onclick=clickFunc;
      }


	
