使用RegExp自带的属性
==
RegExp有几个出场率很少到属性，但利用这些属性可以让代码更快些。


當初[f5](github.com/island205/f5)有個bug：
對於html文件，f5需要在`</body>`前加入兩個`<script>`標籤（socket.io.js和refresh.js）。最早的方案是這樣的[f5.coffee](https://github.com/UncleBill/f5/blob/6c161175cf00c9d2b9f9d576668630e95b06c182/src/f5.coffee#L19)（[diff](https://github.com/UncleBill/f5/commit/6c161175cf00c9d2b9f9d576668630e95b06c182))：
```coffee
insertSocket=(file)->
        index=file.indexOf "</body>"
        if index is -1
                file+=SOCKET_TEMPLATE
        else
                file=file.slice(0,index)+SOCKET_TEMPLATE+file.slice(index)
                file
```
这里隱藏着一個bug，當html中包含`</body>`字符串時(如再js中，文本中)，再插入`<script>`html就会被破坏。
第一次修复的方案是([diff](https://github.com/UncleBill/f5/commit/04142e3b0229815ed5e0233fc06e68b51addb24b))
[f5.coffee](https://github.com/UncleBill/f5/blob/04142e3b0229815ed5e0233fc06e68b51addb24b/src/f5.coffee#L21)
```coffee
insertTempl = (file, templ)->
    matchrx = /<\/\s*body\s*>/gi
    matchs = file.match( matchrx )
    matchs = matchs.concat [""]  # hack for for loop:
                                 # make matchs' length same to splits'
    splits = file.split( matchrx )
    l = splits.length
    _templ = []
    got = ""
    for ii in [0..( l-1 )]
        if ii == l - 2      # final match is coming
            _templ = templ
        else
            _templ = []
        got += splits[ii] + _templ.join("") + matchs[ii]
    got
```
使用`/<\/\s*body\s*>/gi`提高了代码容错率。
第二次的修改用到里新到正则表达式，使代码变简单了：
[diff]()
```coffee
insertTempl = (file, templ)->
    matchrx = ///
    </\s*body\s*>
    (?![^]*</\s*body\s*>)           # not followed by any more </\s*body\s>
    ///gi
    index = file.search matchrx
    if not index
        file += templ.join ''
    else
        file = file[0...index] + templ.join('\n') + file[index...]
```
再使用`replace`
``` coffee
insertTempl = (file, templ)->
    matchrx = ///
    </\s*body\s*>
    (?![^]*</\s*body\s*>)           # not followed by any more </\s*body\s>
    ///gi

    if not file.search matchrx
        file += templ.join ''
    else
        file.replace matchrx, (match, offset, str)->
            "#{templ.join('\n')}\n#{match}"
```
今天看到一个正则到[cheat sheet](http://www.visibone.com/regular-expressions/)，发现还可以这样用正则:
```coffee
insertTempl = (file, templ)->
    matchrx = ///
    </\s*body\s*>
    (?![^]*</\s*body\s*>)           # not followed by any more </\s*body\s>
    ///gi

    if not file.search matchrx
        file += templ.join ''
    else
        RegExp.leftContext + templ.join('\n') + '\n' + RegExp.lastMatch + RegExp.rightContext
```
`Regexp.lastmatch`是最后一个匹配到的项，`Regexp.leftcontext`所匹配位置之前的字符串段，`Regexp.rightContext`所之后的字符串段。利用三个变量就省去里`replace`，想必效率会高些。

写了个简单的测试：

使用到`replace`的是insertTempl1，`insertTempl2`是用了`RegExp`的三个属性
```coffee
htmlcode = fs.readFileSync("./test.html").toString()
times = 10000
console.log(times, "times")

tester = (func, name) ->

    console.time(name)
    for i in [0..times]
        func(htmlcode,templ)
    console.timeEnd(name)

tester(insertTempl1, "func1")
tester(insertTempl2, "func2")
```
结果是：
```
10000 'times'
func1: 22ms
func2: 46ms

20000 'times'
func1: 40ms
func2: 87ms

```
速度明显快里许多

其实，那三个属性还可以这样表示：

```javascript
RegExp.lastMatch == RegExp.$&
RegExp.leftContext == RegExp.$`
RegExp.rightContext == RegExp.$'
```

除此之外还有

```javascript
RegExp.index
RegExp.input == RegExp.$_
Regexp.lastIndex
Regexp.lastParen == RegExp.$+
$1-$9
```

参见：
---
- http://msdn.microsoft.com/en-us/library/ie/9dthzd08(v=vs.94).aspx
- ttp://www.visibone.com/regular-expressions/
- 则可视化工具：http://www.regexper.com/

tags
--
**javascript** **regex**
