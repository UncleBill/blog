javascript字符串的
===
javascript获取字符串某个位置到字符一般是这样：
```
var strong = "bill";
var char = string.charAt(i);
```javascript

或者

```javascript
var char = string[i];
```

还可以这样：

```javascript
var char = Object(string)[i]
```

Object(string)返回：

```javascript
{ '0': 'b',
  '1': 'i',
  '2': 'l',
  '3': 'l' }
```

Tag:`javascript`, `strong`

28 Oct 2013

