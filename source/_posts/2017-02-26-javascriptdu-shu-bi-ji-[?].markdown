---
layout: post
title: "javaScript读书笔记(一)"
date: 2017-01-26 12:08:03 +0800
comments: true
categories: 读书笔记
tags: javaScript
---
虽然写过一点前端的项目，但是一直在别人搭好的框架上进行coding。前端知识并不好，基础不牢总是要付出代价的TT.从头开始补基础，从javaScript开始。<!--more-->

####设计意图
javaScript最初设计意图就是为了在浏览器端中载入的Web页面和位于服务器上的应用程序之间提供脚本化的接口。

####阅读书目

《javaScript学习指南》

####基础知识摘要

最好保持脚本位置的一致性，要么全部放在head元素中，要么全部放在body元素的最末尾处

常见事件处理程序

	onclick     点击事件
	onmouseover 当鼠标停留在某元素上触发
	onmousemout 当鼠标离开某元素时触发
	onfocus     当某元素获得焦点时触发


* document对象的所有目的时呈现整个页面，包括页面中的所有元素；
* 脚本引用example：<script type="text/javascript" src="test.js"></script>
* 转义符 反斜杠 \ 
* 对于非ascii码－－encodeURI/encodeURIComponent对字符串进行编码，转换成URIencoding字符（decodeURI/decodeURIComponent）
* `双重否定符`（!!）可以用来显示地将数字或字符串转换为布尔值
*  javascript`除法`得到的结果是浮点型数字

####Boolean对象
*  `Boolean对象`实例，对象初始值为`空字符串`时候则对象的初始值将为`fasle`,任何`非空字符串`创建boolean对象实例初始值为`true`.

####String对象
* 常用方法
<table>
        <tr>
            <th>方法</th>
            <th>描述</th>
            <th>参数</th>
          </tr>
        <tr>
            <th>contact</th>
            <th>连接字符串</th>
            <th>字符串参数，把该字符串连接到string对象的字面量字符串</th>
        </tr>
        <tr>
            <th>split</th>
            <th>根据特定的分割符，字符串分割</th>
            <th>分割符和分割最大数目</th>
                 </tr>
        <tr>
            <th>slice</th>
            <th>返回字符串的某个片段</th>
            <th>该片段的起始和结束位置</th>
                  </tr>
                    <tr>
            <th>toLowerCase
            toUpperCase</th>
            <th>大小写转换</th>
            <th>无</th>
                  </tr>
    </table>

####正则表达式和RegExp
* RegExp方法：test匹配考虑大小写，加i则忽略大小写,g表示全局匹配 /xxx/ig exec返回结果是一个数组，并存储圆括号包含的子字符串
* ＊表示前面*字符出现零次或多次；+前面字符出现一次或多次；？字符出现0次或1次；.表示字符只出现一次;^脱字符［^0-9］表示非数字字符; ^和$表示一行的开始和结束;\w匹配任何数字字母字符，包括下划线[A-Za-z0-9];｜表示可以选择;｛｝表示前面字符重复的次数

####FIFO队列
	shift方法   移除数组第一个元素 
	unshift方法 将元素添加到数组开