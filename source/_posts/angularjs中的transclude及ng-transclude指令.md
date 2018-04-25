---
title: transclude及ng-transclude
---
> 随笔，小记。。

### transclude？

嵌入的意思,也就是说你是否需要将你的指令（内部）的元素嵌入到你的（模板）中去，默认为false。如果你需要的话，那么就需要将transclude设置为true。如果将这个值设置为true的话，就要配合angular的ng-transclude指令来进行使用，

------
### 代码实现？
1.html中的代码
```
<!--  指令a-transclude 内部含有元素-->
<breadcrumb>指令（内部）内容</breadcrumb>
```
2.js中的代码
```
myApp.directive('breadcrumb', ['$parse', function($parse) {
		return {
			templateUrl: 'tmpls/breadcrumb.html',
            transclude:true
		}
}]);

```

3.tmpls/breadcrumb.html模板中的代码
```
<ol>
  <li>
  	<a href="#" ng-transclude>模板1</a>
  	<a href="#">模板2</a>
  </li>
</ol>
```
备注：*** 模板由两部分组成，一部分是含有ng-transclude指令的，一部分是不含有这个指令的 ***

*** 最后的结果是 ***

(https://blog.yoyoyuan.top/img/transclude.png)

------
