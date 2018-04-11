---
title: 牛客网刷题错误集锦
date: 2018-04-04 15:07:03
tags: 
      --业余学习
categories: JavaScript
---
最近在牛客网上刷题，发现自己还有很多的知识盲点，感觉拿起小本本记录下来，争取早日攻克！
<!-- more -->
1. 数组赋值问题
    ~~~js
    var arr = [];
    arr[0] = 0;
    arr[1] = 1;
    arr.foo = 'foo';
    console.log(arr.length);//2
    ~~~
    原本以为控制台会报错或者输出为3，打印输出arr的值为[0,1,foo:'foo']。了解到length返回的是数组的数字索引长度，故arr.length为2。

2. 闭包问题
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
    nAdd为全局函数，但同时赋值在f1内部，故可以访问堆中的n值，result赋值为f1返回的f2，当运行result()时会直接从堆中获取到n的值。
3. es6语法问题
    import f from 'filename'时，filename文件中必须是export default f;
    import { f } from 'filename'时，filename文件中才可以是export { f };
    即Import不加大括号是导入default的方法。
    另外，es6中export var f = 'sue';等价于var f = 'sue';export { f }; export不仅可以导出变量，同时还能导出函数和类。
4. new obj()
    ~~~js
    var F=function(){};
    Object.prototype.a=function(){};
    Function.prototype .b=function(){};
    var f=new F();//f能拿到a但不能拿到b，因为f instanceof object是true，而f instanceof function 是false
    ~~~
5. call、apply、bind
    这三个用法非常相似，将函数绑定到上下文中，用来改变函数中this的指向。但三者也有所区别。
    call、apply只是临时借调函数，立即执行，bind却只是创建函数不执行（且该新函数永久绑定this）
    ~~~js
    function pay(base, bonus) {
      console.log(this.name + '总工资:' + (base+bonus));
    }
    var liLei = { name: 'liLei' };
    var hanMeiMei = { name: 'hanMeiMei' };
    pay.call(liLei, 10000, 3000);//liLei总工资:13000
    pay.apply(hanMeiMei, [8000, 2000]);//hanMeiMei总工资:10000
    var liLei_pay = pay.bind(liLei, 20000);//绑定pay函数的this指向为liLei,基础工资为20000
    liLei_pay(2000);//liLei总工资:22000，this指向liLei,base绑定20000,传入的第一个数字参数则为奖金参数
    liLei_pay.call(hanMeiMei, 8000, 2000);//liLei总工资:28000
    ~~~
    从这个实例我们可以看出，call和apply的区别是，call的参数是逐个传递，而apply则是以数组形式传递，但结果是相同的,bind方法传递给调用函数的参数可以逐个列出，也可以写在数组中。而bind绑定this为liLei后，则永久绑定了，无论之后怎么调用传入this对象都还是指向绑定的this对象。
