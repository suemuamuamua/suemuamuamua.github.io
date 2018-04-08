---
title: 牛客网刷题错误集锦
date: 2018-04-04 15:07:03
tags: 
      --业余学习
categories: JavaScript
---
最近在牛客网上刷题，发现自己还有很多的只是盲点，感觉拿起小本本记录下来，争取早日攻克！
<!-- more -->
1.数组赋值问题
~~~js
var arr = [];
arr[0] = 0;
arr[1] = 1;
arr.foo = 'foo';
console.log(arr.length);//2
~~~
原本以为控制台会报错或者输出为3，打印输出arr的值为[0,1,foo:'foo']。了解到length返回的是数组的数字索引长度，故arr.length为2。

2.闭包问题
~~~js
function foo() {
  var i = 0;
  return function() {
    console.log(i++);
  }
}
var f1 = foo();//var i=0
var f2 = foo();//再一次var i=0
f1(); //0
f1(); //1
f2(); //0
~~~
函数定义有两种方式，第一种是function name() {},另外一种是var name = function() {},此处定义f1、f2时默认采取的第二种。
出错问题在于f1、f2为两个不同的函数，f1、f2第一次执行的时候局部作用域没有i变量，故向上级作用域找i变量，所以f1、f2中i的初始值均为0。
同类的问题还有：
~~~js
function f1(){
　　　　var n=999;
　　　　nAdd=function(){n+=1}
　　　　function f2(){
　　　　　　alert(n);
　　　　}
　　　　return f2;
　　}
　　var result=f1();
　　result(); // 999
　　nAdd();
　　result(); // 1000
~~~
nAdd为全局函数，但同时赋值在f1内部，故可以访问堆中的n值，result赋值为f1返回的f2，当运行result()时会直接从堆中获取到n的值