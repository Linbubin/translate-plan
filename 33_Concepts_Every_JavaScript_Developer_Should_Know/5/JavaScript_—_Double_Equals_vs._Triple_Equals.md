[原文](https://codeburst.io/javascript-double-equals-vs-triple-equals-61d4ce5a121a)

## The Problem
## 问题
js有两个`看起来`相似却又不太相同的比较相等的方法.你可以用 == 或者===比较相等.下面是他们的不同之处:
JavaScript has two `visually` similar, yet very different, ways to test equality. You can test equality with == or ===. Here are the differences:

## Triple Equals
## 三等于
当时我们在js中要比较严格相等时,使用 ===.这意味着我们比较的值和类型都必须完全相等.
When using triple equals === in JavaScript, we are testing for strict equality. This means both the type and the value we are comparing have to be the same.
让我们来看`几个`完全相等的`例子`.
Lets look `at a couple examples of` strict equality.

在第一个例子,我们比较数字5和数字5.果不其然,返回了true.两者都是5,类型都是number.
In this first example we’re comparing the number 5 with the number 5. As expected, true is returned. Both are numbers, and both share the same value of 5.

```js
5 === 5
// true
```
`记住这一点`,我们再看`另外两个`返回true的`例子`
`With this in mind`, we can look at `two more examples` that will return true:

```js
'hello world' === 'hello world'
// true (Both Strings, equal values)
true === true
// true (Both Booleans, equal values)
```

很好,现在看下返回false的其他例子.
Awesome. Now lets take a look at some examples that will return false:

在这个例子中,我们比较数字77和字符串'77'.这意味着我们`操作数`将会有不同的类型但是相同值.这将会返回false
In this example we’ll compare the number 77 to the string value of 77. This means our `operands` will have the same value, but a different type. This will return false

```js
77 === '77'
// false (Number v. String)
```

这是两个`补充`的例子
Here are two `additional` examples:

```js
'cat' === 'dog'
// false (Both are Strings, but have different values)
false === 0
// false (Different type and different value)
```

很好!继续,我们使用严格等于来比较,必须保证类型和值都相等.
Awesome! Again, the key `takeaway for` triple (strict) equality is that both the type and the value we are comparing have to be the same.

## Double equals
## 双等于
当我们在js中比较`不那么严格`的相等,我们使用双等于.双等于也需要类型`强制`相等.
When using double equals in JavaScript we are testing for `loose` equality. Double equals also performs type coercion.

类型强制意味着先试图将他们`转变`成相同类型后再进行比较.
Type coercion means that two values are compared only after attempting to `convert` them into a common type.

一个例子将说明这个. 回顾一下之前我们用严格等于的时候:
An example will illustrate this. Recall earlier when we tested the following with strict equality:

```js
77 === '77'
// false (Number v. String)
```

77不严格等于'77'是因为他们有不同的类型.然后,当你对他们使用不严格等于...
77 does not strictly equal '77' because they have different types. However, if we were to test these values with loose equality…

```js
77 == '77'
// true
```

可以看到,我们得到true.这是因为类型强制.事实上,js将会试图将值转换成一个类似的类型.‘77’会被很容易的转换成77.然后和77比较,就得到了true这个答案.
You can see we get true. That because of type coercion. JavaScript will actually try to convert our values into a like type. In this case, it succeeds. The string value of '77' can easily be converted into the number value of 77. Since 77 equals 77, we get our answer of true.

再看看其他的例子.
Lets look at one more example.

回顾之前我们严格等于比较false和0:
Recall earlier when we tested with strict equality if false equals 0:

```js
false === 0
// false (Different type and different value)
```
这显然是false. 然后,如果我们用不严格等于来比较相同值...
This is obviously false. However, if we run the same equation with loose equality…

```js
false == 0
// true
```

我们得到了 true? 为什么会这样?这和js的`伪值`有关.我们将会在下个部分来`探索`这个`概念`.
We get true? Why is this? It has to do with `falsy values` in JavaScript. We’ll `explore` this `concept` in the next section.

## Falsy Values
## 伪值
好了,所以为什么 false == 0. 这有点复杂,因为在js中,0是一个伪值.
Okay, so why does false == 0 in JavaScript? It’s complex, but it’s because in JavaScript 0 is a falsy value.

事实上,类型强制会把0转换成false,然后false就等于false了.
Type coercion will actually convert our zero into a false boolean, then false is equal to false.

你需要知道js中有6个伪值:
There are only six falsy values in JavaScript you should be aware of:

* false — boolean false
* 0 — number zero
* “” — empty string
* null
* undefined
* NaN — Not A Number

## Falsy Value Comparison
## 伪值比较

下面你可以认识到伪值的规则.如果你要经常使用js,这些都是你需要xxx的.
The following you can consider to be ‘rules’ of falsy values. These are things you should ultimately memorize if you will be working with JavaScript often.

1. false, 0, and ""
当你用不严谨的等于对三者比较时,总会返回相等.这是因为他们都会转化为false.
When comparing any of our first three falsy values with loose equality, they will always be equal! That’s because these values will all coerce into a false boolean.
```js
false == 0
// true
0 == ""
// true
"" == false
// true
```

2. null and undefined
当比较null和undefined时,他们只会和自己或者他俩互相相等.
When comparing null and undefined, they are only equal to themselves and each other:

```js
null == null
// true
undefined == undefined
// true
null == undefined
// true
```
如果你想要比较null和其他值,将会返回false.
If you try to compare null to any other value, it will return false.

3. NaN
最后NaN不会和任何东西相等,更酷的是,他不会和自己相等.
Lastly, NaN is not equivalent to anything. Even cooler, it’s not even itself!

```js
NaN == null
// false
NaN == undefined
// false
NaN == NaN
// false
```

## Key Takeaways
* 正如你所看到的, 在js中类型强制会变得有点疯狂.除非你对js非常熟悉,弱等于相对于他的价值来说会导致很多头疼.记住6个伪值和与之相关联的规则,就能够很好的理解弱等于.
* As you’ve seen, type coercion can get a bit crazy in JS. Unless you’re very familiar with JavaScript, loose equality can lead to more headaches than it’s worth. Memorizing the six falsy values and the rules associated with them can go a long way towards understanding loose equality.
* 全等于要`优于`弱等于. 无论何时,你都尽可能的用强等于去进行相等比较.通过比较类型和值,你可以很明确,你总是执行一个公平的比较.
* Triple Equals is `superior` to double equals. Whenever possible, you should use triple equals to test equality. By testing the type and value you can be sure that you are always executing a true equality test.