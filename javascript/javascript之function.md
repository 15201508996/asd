
 function
==========
javascript中function,表面看上去没什么特别，其实是一个对象。
-----------

### 1. function  结构
   >arguments
   >>在函数代码中，使用这个对象就能访问参数,
   >caller
   >> 返回一个对函数的引用，该函数调用了当前函数。
   >> 对于函数来说，caller属性只有在调用的时候才会存在。
   >> 如果函数是顶层调用，caller就是null。
   >> 相区别于arguments.callee,callee返回当前函数的引用
   >length
   >> 只读属性，函数需要实参数目，也就是在函数的形参列表中申明的数目。
   >name 
   >> 函数名称
   >\__proto__
   >> 对象原型链基础函数，默认一个空函数
