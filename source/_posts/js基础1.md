---
title: js超基础
---

突然发现对js的一些细节知识点记忆几乎遗忘，所以从今天开始我想从新
把js基础给过一边，来来巩固一下。

> * JavaScript和ECMAScript的关系
> * 程序书写的位置、简单语句
> * 类型、值、变量
> * 表达式、运算符


------
>## 一、JavaScript和ECMAScript的关系

对于他们的比较，只要知道一句就好了：简单来说ECMAScript不是一门语言，而是一个标准，是js的一个标准。而javascript是一种轻量级语言。
-------

>## 二、程序书写的位置、简单语句

### ***2.1程序书写的位置***
1. 行内式：
```pathy
    <button onclick="alert('你好吗')">点击我</button>
```
    一般情况，单双引号是一样的,但是出现了包裹的情况,所以要严格处理这种情况，我们一般采取的是 外双内单的格式。
```pathy
<a href="javascript:void(0);"></a>  // 防止跳转
```
2. 内联式：
```pathy
<script type="text/javascript"></script>  //任何一个地方
```
最好放在body标签下面，否则会出现加载顺序的问题。如果要写在head标签里的话需要些一个入口函数：
```pathy
  <script type="text/javascript">
        window.onload = function(){ 
            alert("hello")
        }
  </script> 
```
3. 外链式：
另外建立一个文件，然后以.js为后缀名，里面放你要写的js代码。然后再html里面引入一下代码：
```pathy
<script type=”text/javascript” src=”js的路径.js”></script>
```

### ***2.2简单语句***
1. alert（）语句：
```pathy
        <script type="text/javascript">
      window.alert("今天真好");
            /* 这里说明一下window是可以省略的 */
    </script>    /*（内联式）*/
```
这是一个弹出框语句，打开页面后，会有一个弹出框。alert就是英语里面的“警报”的意思，用途就是弹出“警告框”。
2. prompt（）语句：
```pathy
    <script type="text/javascript">
    var a = prompt("请输入");
    alert(a);
  </script>   
```
prompt就是专门用来弹出能够让用户输入的对话框
3. confirm（）

  comfirm(“你好吗？”);//消息对话框通常用于允许用户做选择的动作
消息对话框通常用于允许用户做选择的动作，点击确定，返回true,点击返回，返回false.
4. console控制台输出

Console.log() 控制台输出 普通输出语句
Console.warn()  控制台警示
Console.error() 控制台错误提示
Console.info()  一般
Console.debug() 除错信息
5. document.write()
document.write()直接在HTML文档中写入内容并显示

------

>## 三、 类型、值、变量
### ***3.1类型***
1.  * 简单的数据类型：
     * number     数字类型       
     * string     字符串类型     
     * boolean    布尔类型  

     * undefined 
在使用var声明变量，但未对其加以初始化时，这个变量的类型就是undefined，且其默认初始化值为undefined。
对未声明与初始化的变量，直接使用，那么这个变量的类型也是undefined，但是没有默认初始化值。
     * null
null类型的默认值是null，从逻辑角度讲，是表示一个空对象指针。
js高级程序上有讲到，undefined类型是派生自null的，不严格的说二者都是指没有明确赋值的类型，但是细分之后，undefined类型，被用来形容未经初始化的变量，null类型被用来形容空对象指针。
所以，如果定义的变量准备在将来用于保存对象（即复杂的数据类型object），那么就该将该变量初始化为null。
区分，当一个变量声明后，未初始化，则该值为undefined，如果这个值是为了保存对象，则修改其默认初始化的值，改为null。 所以当检测其类型时，会显示类型为object。

文／Miss____Du（简书作者）
原文链接：http://www.jianshu.com/p/4841fcc6b4e7
著作权归作者所有，转载请联系作者获得授权，并标注“简书作者”。
    * 复杂的数据类型
     * object   对象
2. 数据类型转换：
### 隐式类型转换
    数字-->字符串：
```pathy
   var a = 123;
   a  = a + "";
   console.log(typeof(a))
```
字符串-->数字
```pathy
   var a = "123";
   a  = a - 0 ;
   console.log(typeof(a))
```
### 强制类型转换
数字-->字符串
```pathy
var a = 123;
var b = string(a)
   console.log(typeof(b))
 //或用a.toString();
```
字符串-->数字
```pathy
var a = "123";
var b = Number(a)
   console.log(typeof(b))
```
转换成整型：parseInt()；
转换成浮点型： parseFloat();
### ***3.2变量***
1.  变量的命名规范：
只能由字母、数字、下划线、美元符号$构成，且不能以数字开头，并且不能是JavaScript关键字保留字。
比如：
var a ;
var $ab123;
2. 注意要点，
    * 变量用var来定义。只有定义之后，这个变量才能够使用，而且不需要考虑他到底是什么类型的，尽管用var来第一即可。
    * 后面代码要用到变量的时候一定不要加引号，加引号就变成了字符串了，而不是变量。
    * 大写字母是可以使用的，并且大小写敏感。也就是说A和a是两个变量。
    * 变量名不能超过255个字符，一半也不会写那么长。。
    * 如果在声明时没有写var被认为是全局变量。
    * javascript里面声明的变量，会提升至函数顶部。
    
------

>## 四、语句
* if语句
```pathy
  if(表达式){
    console.log(我就出去玩)；
  }else{
    console.log(我就在家写作业); 
    }

```
* for循环语句
```pathy
  for(var i = 1 ; i <= 100 ; i++){
    console.log(i);
  }

```
* switch语句
```pathy
var n = null;
switch(n){
    case 1:
      执行的代码;
        break;
    case 2:
      执行的代码;
        break;
        ......
    default:
      执行的代码;
        break;
  }

```
* while语句
```pathy
var i = 1；
while（i<10）{
    console.log(i)
    i ++ ;
}
```
* Do..while循环语句
```pathy
var i = 1；
do{
 console.log(i)
    i ++ ;
}while(i<10)
```
* break 语句 和 continue语句
break语句：它是结束当前整个这个循环体，跳出该循环体；
continue语句：它是结束当前的一个循环，后面的内容不执行，跳入本循环的下一个循环。

>## (扩展小知识点)序列化和反序列化
主要用于存储对象状态为另一种通用格式，比如存储为二进制、xml、json等等，把对象转换成这种格式就叫序列化，而反序列化通常是从这种格式转换回来。
使用序列化主要是因为跨平台和对象存储的需求，因为网络上只允许字符串或者二进制格式，而文件需要使用二进制流格式，如果想把一个内存中的对象存储下来就必须使用序列化转换为xml（字符串）、json（字符串）或二进制（流）
