# 技巧

下面的技巧，后三个，请谨慎用于团队项目中(主要考虑到可读性的问题)，不然，leader 干你没商量。

Boolean
这个技巧用的很多，也非常的简单

!!'foo'
通过两个取反，可以强制转换为Boolean类型。较为常用。

Number
这个也特别简单，String转化为Number

+'45'
+new Date
会自动转化为number类型的。较为常用。

IIFE
这个其实非常有实用价值，不算是装逼。只是其他语言里没有这么玩的，给不太了解js的同学看那可牛逼大了。

(function(arg) {
  // do something
})(arg)
实用价值在于可以防止全局污染。不过现在随着ES2015的普及已经没什么必要用这个了，我相信五年之后，这种写法就会逐渐没落。

自己干五年，在实习生面前装逼用也是蛮不错的嘛~

Closure
闭包嘛，js 特别好玩的一个地方。上面的立即执行函数就是对闭包的一种运用。

不了解的回去翻翻书，知乎上也有很多讨论，可以去看看。

闭包用起来对初学者来说简直就是大牛的标志(其实并不是)。

var counter = function() {
  var count = 0
  return function() {
  return count++
  }
}
上面用到了闭包，看起来还挺装逼的吧。不过好像没什么实用价值。

那么这样呢？

var isType = function(type) {
  return function(obj) {
  return toString.call(obj) == '[Object ' + type + ']';
  }
}
通过高阶函数很轻松的实现判定类别。(别忘了有判定Array的Array.isArray())

当然，很明显，这只是基础，并不能更装逼一点。来看下一节

Event
事件响应前端肯定都写烂了，一般来说如何写一个计数器呢？

var times = 0
var foo = document.querySelector('.foo')
foo.addEventListener('click', function() {
  times++
  console.log(times)
}, false)
好像是没什么问题哦，但是！变量times为什么放在外面，就用了一次放在外面，命名冲突了怎么办，或者万一在外面修改了怎么办。

这个时候这样一个事件监听代码就比较牛逼了

foo.addEventListener('click', (function() {
  var times = 0
  return function() {
  times++
  console.log(times)
  }
})(), false)
怎么样，是不是立刻感觉不一样了。瞬间逼格高了起来！

通过创建一个闭包，把times封装到里面，然后返回函数。这个用法不太常见。

parseInt
高能预警
从这里开始，下面的代码谨慎写到公司代码里！
parseInt这个函数太普通了，怎么能装逼。答案是~~

现在摁下F12，在console里复制粘贴这样的代码：

~~3.14159
// => 3
~~5.678
// => 5
这个技巧十分装逼，原理是~是一个叫做按位非的操作，会返回数值的反码。是二进制操作。

原因在于JavaScript中的number都是double类型的，在位操作的时候要转化成int，两次~就还是原数。

Hex
十六进制操作。其实就是一个Array.prototype.toString(16)的用法

看到这个词脑袋里冒出的肯定是CSS的颜色。

做到随机的话可以这样

(~~(Math.random()*((1<<24)-1))).toString(16)+'00000').substring(0,7)
底下的原文链接非常建议去读一下，后三个技巧都是在那里学到的。

注：
此随机数会产生五位，四位，甚至一位的颜色。所以严谨的话还是要做一下判定的
谢谢@yangfch3和@scar的勘误
«
左移操作。这个操作特别叼。一般得玩 C 玩得多的，这个操作会懂一些。一般半路出家的前端码农可能不太了解(说的是我 ☹)。

这个也是二进制操作。将数值二进制左移

解释上面的1<<24的操作。

其实是1左移24位。000000000000000000000001左移24位，变成了1000000000000000000000000

不信？

试着在console粘贴下面的代码

parseInt('1000000000000000000000000', 2) === (1 << 24)
其实还有一种更容易理解的方法来解释

Math.pow(2,24) === (1 << 24)
二进制操作。
