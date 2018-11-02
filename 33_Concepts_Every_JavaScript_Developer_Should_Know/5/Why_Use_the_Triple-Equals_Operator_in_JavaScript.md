[原文](https://www.impressivewebs.com/why-use-triple-equals-javascipt/)

# 为什么在js中要使用===操作符?
# Why Use the Triple-Equals Operator in JavaScript?

By Louis Lazaris on March 1st, 2012 | 33 Comments

`确定`两个变量是否相等的是程序中最重要的操作之一. 

Nicholas Zakas在他《JavaScript for Web Developers》中说到.

“`Determining` whether two variables are equivalent is one of the most important operations in programming.” That’s according to Nicholas Zakas in his book JavaScript for Web Developers.

`换句话说`,`贯穿`你的scripts,你将可能会有`类似`以下的代码:

`In other words`, `throughout` your scripts you’ll probably have lines `resembling` this:

```js
if (x == y) {
  // do something here
}
```

或者说,如果你遵循最佳的实践,像这样:

Or, if you’re `conforming` to best `practices`, this:
```js
if (x === y) {
  // do something here
}
```

两个例子不同指出是第二个例子使用全等于操作符,也叫做严格等于或`恒等`.

The difference between those two examples is that the second example uses the triple-equals operator, also called “strict equals” or “`identically equal`”.

试图`坚持`最佳实践的js`初学者`或许会使用全等于而不是双等于,但是并不是完全理解他们之间的区别和为什么坚持全等是重要的.

JavaScript `beginners` who try to `adhere` to best practices may be using triple-equals and not double-equals, but might not fully understand what the difference is or why it’s important to `stick` to triple-equals.

他们有什么区别?

What’s the Difference?

在一个使用双等于操作符的比较中,如果两者比较是相等,结果会返回true.但是这一点很重要的陷阱:如果比较数是不同类型,将会`发生`强制类型转换.

In a comparison using the double-equals operator, the result will return true if the two things being compared are equal. But there’s one important catch: If the comparison being made is between two different “types” of values, type coercion will `occur`.

每个js值都属于一个特殊的类型.有Number,string,Booleans,functions和object类型.所以如果你试图比较string和number类型,浏览器会在比较前尝试将string转化成number类型.相同的,如果你比较boolean和number类型,boolean会转化成1或者0.

Each JavaScript value belongs to a specific “type”. These types are: Numbers, strings, Booleans, functions, and objects. So if you try comparing (for example) a string with a number, the browser will try to convert the string into a number before doing the comparison. Similarly, if you compare true or false with a number, the true or false value will be converted to 1 or 0, respectively.

这会到来`不可预测`的结果.这是一些例子:

This can bring `unpredictable` results. Here are a few examples:
```js
console.log(99 == "99"); // true
console.log(0 == false); // true
```
虽然在一开始这看起来是个好事情(因为浏览器看起来帮了你一个大忙),但是它会造成一些问题. 比如:

Although this can initially feel like a good thing (because the browser seems to be doing you a favour), it can cause problems. For example:
```js
console.log(' \n\n\n' == 0); // true
console.log(' ' == 0); // true
```
正是如此,大多数js`专家` `推荐`总是使用全等于操作符,从不使用双等于.
In light of this, most JavaScript `experts` `recommend` always using the triple-equals operator, and never using double-equals.

全等操作符会执行你所想的结果,不会进行类型转换.所以无论何时你使用全等于,你都是对`实际`的值做一个`精密`的比较.你可以保证这个值是完全相等或恒等.

The triple-equals operator, as you’ve probably figured out by now, never does type coercion. So whenever you use triple-equals, you’re doing an `exact` comparison of the `actual` values. You’re ensuring the values are ‘strictly equal’ or ‘identically equal’.

这意味着,上面的例子使用全等于会产生`合适`的结果:

This means that, using triple-equals, all the examples from above will produce the `correct` results:
```js
console.log(99 === "99"); // false
console.log(0 === false); // false
console.log(' \n\n\n' === 0); // false
console.log(' ' === 0); // false
```
什么是不相等?

What About Inequality?

当进行不相等表达式时,规则一样适用.除了这个时候,将全等和双等换成双等和单个.

When doing a not-equals-to expression, the same rules apply. Except this time, instead of triple-equals vs. double-equals, you’re using double-equals vs. single.

上面很很多相同的例子,现在解释下 != 操作符:

Here are the same examples from above, this time expressed with the != operator:
```js
console.log(99 != "99"); // false
console.log(0 != false); // false
console.log(' \n\n\n' != 0); // false
console.log(' ' != 0); // false
```
注意现在期望的结果应该是true.然而,他们false,因为类型转换.
Notice now that the `desired` result in each case should be “true”. Instead, they’re false — because of type coercion.

如果我们改成双等于,我们获得合适的结果:

If we change to double-equals, we get the correct results:
```js
console.log(99 !== "99"); // true
console.log(0 !== false); // true
console.log(' \n\n\n' !== 0); // true
console.log(' ' !== 0); // true
```
结论

Conclusion

正如提到的,你可能早就完全使用全等于.当`研究`这篇文章,我学到关于一些事情的`概念`.

As mentioned, you’ve probably already used triple-equals pretty exclusively. While `researching` this article, I learned a few things about this `concept` myself.


我认为最好的总结再次来自Zakas,在推荐总是使用严格相等后,他说: "这将会在你的整个代码中帮助`保持`数据类型的`完整性`"
I think the best summary comes from Zakas again, where, after recommending always using strict equals, he says: “This helps to `maintain` data type `integrity` throughout your code.”