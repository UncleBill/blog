通过`getElementsByTagName`或`getElementsByClassName`获取的元素集，可以像`Array`一样进行索引，同时还有`length`属性，但不是数组对象，不能进行push,pop等数组方法的操作。若想使用数组对象的方法对元素集进行操作，须将元素存入数组中。刚开始学javascript的时候，想到的方法是这样的：

```javascript
var ret = [];
for (var i = 0; i < nodeList.length; ++i) {
    ret.push(nodeList[i])
}
// ret就是想要的数组。
```

后来知道了更简单的方法：

```javascript
Array.prototype.slice.call(nodeList, 0);
[].prototype.slice.call(nodeList, 0);
//甚至更简单
[].slice.call(nodeList, 0);
```
通过call方法，让nodeList可以使用数组对象的方法。

这种方法不仅能应用在nodelist这样的元素集上，还能运用在更多地方：

DOMTokenList
------------
```javascript
var cl = elem.classList;
// cl是一个DOMTokenList对象, 包含元素的类名，格式如：{0: 'classname1', 1: 'classname2', length: 2}
var clArr = [].slice.call(cl, 0);
```

CSSStyleDeclaration
-------------------
```javascript
var styles = window.getComputedStyle(elem, null);
var styleArr = [].slice.call(styles, 0);
// styles中包含了元素渲染后的样式
// 一部分是css属性名称，
// 另一部分是以css属性名为键，属性值为值的简直对
// 在这里，丢失了后一部分。
// 这个方法可以轻松获取浏览器支持的css属性
```


tags
--
**javascript** **Array** **call()**
