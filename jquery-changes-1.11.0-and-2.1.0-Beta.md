（未完成）

jQuery发布1.11.0/2.1.0 Beta版，跟着Changelog看一遍被修正的bug
http://blog.jquery.com/2013/11/15/jquery-1-11-02-1-0-beta-2-released/

Ajax
====
- [#14036: ajaxLocation Includes HTTP Basic Authentication Info](http://bugs.jquery.com/ticket/14036)

--
    patch的作者在[PR](https://github.com/jquery/jquery/pull/1340)中给出了这样的例子

        You can see an example of this behavior by visiting http://username:password@example.com/ in Chrome and running location.href. Running the following with this address

        ajaxLocParts = rurl.exec( ajaxLocation.toLowerCase() ) || [];

        will produce username as the domain instead of example.com

    `http://username:password@example.com/`就是bug出现的例子

    [Changes:](https://github.com/jquery/jquery/pull/1340/files)

用`regexper`做了下比较：

    - [2ac1cd91db5c20cbe8ba0db44f7640cd150060ee](http://www.regexper.com/#%5E(%5B%5Cw.%2B-%5D%2B%3A)(%3F%3A%5C%2F%5C%2F(%3F%3A%5B%5E%5C%2F%3F%23%5D*%40%7C)(%5B%5E%5C%2F%3F%23%3A%5D*)(%3F%3A%3A(%5Cd%2B)%7C)%7C))

    - [njhamann/243d3189a1daf805a56912c811af00e7cca35866](http://www.regexper.com/#%5E(%5B%5Cw.%2B-%5D%2B%3A)(%3F%3A%5C%2F%5C%2F(%3F%3A%5B%5E%5C%2F%3F%23%5D*%40%7C)(%5B%5E%5C%2F%3F%23%3A%5D*)(%3F%3A%3A(%5Cd%2B)%7C)%7C))

    patch是`//`之后增加了`(?:[^\/?#]*@|)`

- [#14356: Remove string indexing used in AJAX](http://bugs.jquery.com/ticket/14356)

--
[PR](https://github.com/jquery/jquery/pull/1308) 和
[chages](https://github.com/jquery/jquery/pull/1308/files)

很简单的bug，ie6不支持`str[0]`，解决方案是使用`str.charAt(0)`


- [#14379: Issue with xhr.js](http://bugs.jquery.com/ticket/14379)

--
[f9d41ac641dcb5a93ba8a9027476b160d8f41111](https://github.com/jquery/jquery/commit/f9d41ac641dcb5a93ba8a9027476b160d8f41111)

不是很明白

Attributes
==========

这部分可以跳过

- [#14250: addClass and removeClass needlessly assign to className.](http://bugs.jquery.com/ticket/14250)

--

[commit](https://github.com/jquery/jquery/commit/c418b94eb48188cd9329519ae5e030a52dd81cc9)
如果元素当前的`className`和计算出来的一样，就不必修改，避免不必要的渲染


Build
=====
- [#12757: Enforce style guide via build process](http://bugs.jquery.com/ticket/12757)

[commit](https://github.com/jquery/jquery/commit/5ce0b342577076a4355ba1bb0ad0aef98261f236)

`.jscs.json`和`Guntfile.js`的修改

- [#13983: Switch to //# for sourcemap directives](http://bugs.jquery.com/ticket/13983)

[commit](https://github.com/jquery/jquery/commit/d53ddc90c1f119fb9148a553443ef3fbc3f3cc99)
对`sourcemap`的`//@`符号的修改，改成 `//#`

- [#14113: AMD-ify jQuery source](http://bugs.jquery.com/ticket/14113)
[commit](https://github.com/jquery/jquery/commit/6318ae6ab90d4b450dfadf32ab95fe52ed6331cb)

修改真多！

- [#14118: Use bower to include Sizzle and QUnit (remove submodules)](http://bugs.jquery.com/ticket/14118)

[commit](https://github.com/jquery/jquery/commit/b13d8229ae25a6c48b1d59a0e592cde154e063f0)

- [14163: Make Deferreds/Callbacks/.ready() optional modules](http://bugs.jquery.com/ticket/14163)
[commit](https://github.com/jquery/jquery/commit/6318ae6ab90d4b450dfadf32ab95fe52ed6331cb)

跟#14113是一起被修改

- [14415: Remove sourcemap comment](http://bugs.jquery.com/ticket/14415)

去掉jQuery.js中的`source map`

[commit](https://github.com/jquery/jquery/commit/562145e887cc42ce8c9c9cd2c6e946ff01e6731d)

- [14450: Remove CommonJS+AMD syntax from source](http://bugs.jquery.com/ticket/14450)

[commit](https://github.com/jquery/jquery/commit/a5037cb9e3851b171b49f6d717fb40e59aa344c2)

Core
====
- [14164: Reduce forced layout reflows in init or methods](http://bugs.jquery.com/ticket/14164)
[c418b94eb48188cd9329519ae5e030a52dd81cc9](https://github.com/jquery/jquery/commit/c418b94eb48188cd9329519ae5e030a52dd81cc9)
跟#14250是一起被修改

- [14492: parseJSON incorrectly accepts comma expressions](http://bugs.jquery.com/ticket/14492)

css
===
- [#14394: style=”x: y !important;” doesn’t get changed when calling el.css(x, z) in Chrome and Safari but it works in Firefox](http://bugs.jquery.com/ticket/14394)

    像标签内含有`style="x: y !improtant"`,通过`elem.style.x = z`是无法对其进行修改的

    [pull/1385](https://github.com/jquery/jquery/pull/1385/files)提供的patch是修改style.x前，先清空style.x

- 



Data
====

- [#14101: JQUERY 1.10′S .DATA() RESULT DIFFERS FROM 1.8 WHEN ATTEMPTING TO GET DATA FROM A NON-EXISTENT OBJECT.](http://bugs.jquery.com/ticket/14101)
- [#14459: data-* attribute parsing bypasses jQuery.parseJSON (inconsistent with 1.x)](http://bugs.jquery.com/ticket/14459)

Effects
=======

- [#14344: Putting different effects in callbacks uses only the first effect.](http://bugs.jquery.com/ticket/14344)
Event
=====

- [#13993: .triggerHandler doesn’t return value from handler for DOM0 events](http://bugs.jquery.com/ticket/13993)
- [#14180: focusin/out special events don’t work cross-window](http://bugs.jquery.com/ticket/14180)
- [#14282: Don’t call getPreventDefault() if there is a defaultPrevented property](http://bugs.jquery.com/ticket/14282)
Selector
========

- [#14142: Wrong number of elements returned in XML document with numeric IDs in Safari](http://bugs.jquery.com/ticket/14142)
- [#14351: Exception thrown when running `find` in a non-attached DOM node](http://bugs.jquery.com/ticket/14351)
- [#14535: Selection fails in IE11 when the last context is a no-longer-present iframe document](http://bugs.jquery.com/ticket/14535)
Support
=======

- [#10814: make support as lazy as possible with closure in mind](http://bugs.jquery.com/ticket/10814)
- [#14084: elem.css(‘width’) provides incorrect output with `box-sizing: border-box` if run before document ready](http://bugs.jquery.com/ticket/14084)
- [#14401: Error when loading a page with application/xhtml+xml](http://bugs.jquery.com/ticket/14401)
- [#14496: jQuery 2.1.0-beta1 fails to initialize in a XHTML page](http://bugs.jquery.com/ticket/14496)
