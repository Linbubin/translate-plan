[原文](https://bytearcher.com/articles/equality-comparison-operator-javascript/)

# Should I use === or == equality comparison operator in JavaScript?
# 在js中比较相等时,我应该使用==还是===?

你知道js中有两种不同的相等比较操作符: == 和 ===,他们分别叫做 全等于和双等于.你经常使用他们,但是在写代码时不能够很明确的知道你自己要选择哪一个.你需要知道一个很明确的方法来决定你选择哪个操作符.
You know there are two different equality comparison operators in JavaScript: the === and == operators, or the triple equals and double equals as they're called. You’ve seen both used but aren't sure which one to pick for your code. You'd like to know a sure fire way to decide over one or the other.

事实表明,这有一个`直接`的方法来决定用哪一个.相同的`逻辑`也可以在比较不相等时判断使用!==和!=.
As it turns out, there is a `straightforward` way to decide which one to use. And the same `logic` works for inequality comparison operators !== and != as well.

比较同类型是否相等时使用 ===
Compare equal and of same type with ===

```js
1 x === y
```

当比较相同类型和相同值时, === 会返回true.如果比较不同的类型,结果会是false.
The triple equals operator (===) returns true if both operands are of the same type and contain the same value. If comparing different types for equality, the result is false.

这种定义对于大多数`用例`来说是足够的.当比较0和'0'时,结果会按预期的一样返回false.
This definition of equality is enough for most `use cases`. When comparing the string "0" and the number 0 the result is false as expected.

MDN和D. Crockford都建议,在js中尽可能的使用===操作符,==操作符应当被完全忽略.
Sources such as D. Crockford and MDN both advise that only triple equals operator should be used when programming in JavaScript and double equals operator be ignored altogether.

复杂的类型转换使用 ==
Complex type conversions take place with ==

```js
1 x == y
```

当比较数的类型不同时,==会先进行类型转换,然后再比较值.当类型不同时,操作数会转化成一个统一的类型.转换规则是复杂的,而且依据`参数`类型.关于一个完整的比较表格,看MDN的例子或者是es的`标准`.
The double equals operator (==) tries to perform type conversion if the types differ and then compare for equality. If the types differ, either or both operands are first converted to a common type. The conversion rules are complex and depend on the argument types. For a full comparison table, see for example MDN or the ECMAScript `specification`.

例如,当比较'0'和0时,第一个参数首先被转化为数字类型.
For example, when comparing the string "0" and the number 0, the first argument is first converted to a number.

```js
1 "0" == 0 // becomes
2 ToNumber("0") === 0
```

`虽然`字符串和数字比较是可以被理解的,但是其他不同类型间的复杂规则会导致不符合逻辑的结果.比如比较null,undefined,false之间的关系:
`While` the string and number comparison is understandable, the complex rules for other types lead to illogical results. For example, see the comparisons between null, undefined and false:

```js
1 false == undefined // false
2 false == null      // false
3 null == undefined  // true
```

## Now you know - only use ===
## 现在,你需要记住只是用 ===
使用===当作相等比较符.当比较不同类型时,结果总会返回false.你将总是会得到你预期的结果,而不用去记住转换规则. 就好比比较苹果和香蕉,你会获得false.
Use the triple equals (===) equality comparison operator. When trying to compare different types with triple equals, the result is always false. You will get the results you expect that are `not subject` to hard to memorize conversion rules. Comparing apples to oranges is false as expected.

请记住,这些比较符只比较`原始`类型.如果比较objects和arrays的深层相等,需要一个能比较`结构的`不同的`方法`.
Note that these comparison operators are for comparing `primitive` types. For comparing deep equality of objects or arrays a different `approach` is needed that compares the operands `structurally`.