---

title: js中的集合

---

方法语法：map()
map(callback)
为包装集中的每一个元素调用回调函数，并将返回值收集到jQuery对象的实例中。
参数
callback （函数）回调函数，为包装集中的每个元素调用该函数。
比如，下面的代码将页面上所有div元素的id值收集到一个javascript数组中：
复制代码 代码如下:

var iDs = $("div").map(function(){
    return (this.id==undefined) ? null :this.id;
}).get();
再看如下的表单中包含的一组 checkbox 框：
复制代码 代码如下:
```

<form method="post" action="">
<fieldset>
<div>
<label for="two">2</label>
<input type="checkbox" value="2" id="two" name="number[]">
</div>
<div>
<label for="four">4</label>
<input type="checkbox" value="4" id="four" name="number[]">
</div>
<div>
<label for="six">6</label>
<input type="checkbox" value="6" id="six" name="number[]">
</div>
<div>
<label for="eight">8</label>
<input type="text" value="8" id="eight" name="number[]">
</div>
</fieldset>
</form>

```
我们可以得到一个用逗号分隔的复选框 ID:
复制代码 代码如下:

$(':checkbox').map(function() {
return this.id;
}).get().join();
此调用的结果是字符串， "two,four,six".
在回调函数中，this指向每次迭代中的当前DOM元素。
方法语法：each()
each(iterator)
遍历匹配集里所有的元素，为每一个元素调用传入的迭代函数
iterator （函数）回调函数，为匹配集中的每个元素调用
each()方法也可以用来遍历javascript数组对象甚至单个对象，举个栗子：
复制代码 代码如下:

$([a,b,c,d]).each(function(){
    alert(this);
})
这个语句会为传入$()中数组的每个元素调用迭代函数，函数中的this指向单独的数组项。
每次回调函数执行时，会传递当前循环次数作为参数(从0开始计数)。更重要的是，回调函数是在当前DOM元素为上下文的语境中触发的。因此关键字 this 总是指向这个元素。
假设页面上有这样一个简单的无序列表。
复制代码 代码如下:

<ul>
<li>foo</li>
<li>bar</li>
</ul>
你可以选中并迭代这些列表：
复制代码 代码如下:

$( "li" ).each(function( index ) {
console.log( index + ": "" + $(this).text() );
});
列表中每一项会显示在下面的消息中：
0: foo
1: bar 
两者的区别
map()方法主要用来遍历操作数组和对象，each()主要用于遍历jquery对象。
each()返回的是原来的数组，并不会新创建一个数组。
map()方法会返回一个新的数组。如果在没有必要的情况下使用map，则有可能造成内存浪费。