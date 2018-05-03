---
title: ([].slice.call(arguments, 1))的理解
---

------


> * [] 是什么？
> * [].slice是什么？
> * fn.call()是什么？
> * arguments是什么？


------

## 一. []是什么？


```js
let arr = []                        
let arr1 = new Array()     
```
这两者都是创建一个空数组，没有什么不同的。

------

## 二. [].slice是什么?
它是用来截取数组的.

```js
var a = [1, 2, 3, 4, 5];
var b = a.slice(3);// b是a从2号位开始的片段// 也就是[4, 5]
var c = a.slice(1, 3) // [2, 3]
```

------


## fn.call()是什么？

js一切皆对象，函数也是对象，对象就有一些属性和方法，call就是函数的方法，他是用来改变函数里this指针。
```js
var a = function (n) {    
   console.log(this.n)
}
   
var b = {
  n: 123
};

a.call(b, 2); // log出b对象,123
```

------


## 四. arguments是什么？

arguments是一个数组(并不是真正的数组)。每一个js函数内部都有arguments，它代表传入的参数数组（类数组）。

```js
function a () {

   console.log(arguments) 

}

a(1, 2, 3) // [1, 2 ,3]

```

------


## 五. 来理解[].slice.call(arguments, 1)

直接看下代码：

```js
var a = function(f){   

  console.log([].slice.call(arguments, 1));

}

a('abc', [1, 2, 3, 4, 5]); // 返回[Array[4]]
```
你会想是不是直接这样写就好理解了：arguments.slice(1),其实arguments是个类数组，不是真正的数组，他没有slice方法
```js
var a = function(f){

 console.log(arguments instanceof Array)   

 console.log(arguments.slice(1));

}

a('abc', [1, 2, 3, 4, 5]);

// false

// 结果报错：arguments.slice is not a function...
```
所以在使用slice方法的时候，需要用类似[].slice.call(arguments, 1) 的这种方式去调用
