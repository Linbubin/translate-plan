[原文](https://www.codementor.io/javascript/tutorial/double-equals-and-coercion-in-javascript)

# == vs === JavaScript: Double Equals and Coercion
# js中 == 和 ===的区别: 双等于和强制转换
> #JavaScript   #Coercion    – August 22nd 2016  

==(双等于或者说弱比较)操作符是一个很有意思的操作符.由于大多数人都不太理解它的原理,所以尽可能不用它.但是这不是你不用它的理由,一个好的原因应该是你理解了它,并且你知道为什么不用它.然后,你将会知道在比较时更青睐于使用===.

The == (double equals or loose equality) operator is an interesting operator. Many avoid it because they don’t know how it works. But that’s not a good reason to avoid it; a better reason would be knowing how it works, and knowing why you want to avoid it. And then you will know why you prefer the === (triple equals) operator for comparisons.

在读完这篇文章以后,你会知道js中你需要知道的所有有关强制转换和双等于的知识.让我们开始吧.

After reading this article, you will know all you need to know about coercion and double equals in JavaScript. Let’s get started!


## Double Equals
## 双等于
如果两个比较数的类型不同,双等于会强制转化他们的类型,反之,则会用全等于来进行比较.比如`2==3`,由于2和3的类型相同,比较就会转化为`2===3`.但是如果类型不同,js会强制将一个或者两个的类型转化.比如`2=='3'`,你会比较2和'3'.`在这种情况下`, js将会强制转化'3'为3,然后再比较`2===3`,结果返回false.
The double equals operator coerce the operands if their types are not the same, otherwise, it will use the triple equals operator. So, 2 == 3 is the same as 2 === 3 because both 2 and 3 are of the number type. But if the types are not the same, JavaScript will try to coerce either one or both of the operands. For example, if you compare 2 == "3", you are comparing number 2 with a string whose value is 3. `In this case`, JavaScript will coerce string 3 to number 3, and then it will call 2 === 3 which will result false.

我们已经了解了双等于操作符的基础知识,现在让我们知道深入一下,并且学习操作符将操作数转化为哪种格式.
Now, that we know the basics of the double equals operator, let’s dive deeper and learn how the operator decides what to do.

## The Abstract Equality Comparison
## 抽象的等于比较
(Side note: You can read the complete information about abstract equality comparison in the ECMAScript® 2015 Language Specification)
> 旁注: 你可以在[es2015](http://www.ecma-international.org/ecma-262/6.0/#sec-abstract-equality-comparison)阅读关于抽象的等于比较的完整的信息.

假设我们比较x和y,x和y可以是任意类型:
Assume that we are comparing x and y, where x and y could be of any type:

**首先**:检查是否为同类型.如果是同类型,js就直接使用===来进行比较.

**Start**: Check if the types are the same. If the types are the same, JavaScript will call triple equals and you are done.

但是如果不是同类型,js会遵循另一条路径.js会尝试`找出`怎么强制转换.这是它要执行的步骤:

But if the types are not the same, JavaScript will follow another path. In this path, JavaScript will try to `figure out` what to coerce. These are the steps it will execute:

a. 首先检查我们是不是比较 undefined 和 null,如果是就返回 true,否则:
a. First check if we are comparing null and undefined. If we are, return true. If not:

b. 检查是不是比较 string和number类型.如果是,将string转化成number,然后利用 == 进行比较,再返回到a步骤.比如:
b. Check if we are comparing a string with a number. If we are, coerce the string value to number and call the double equals and go back to the very first step. Eg:
```
2 == "3"
      ↓   coerce "3" to number 3.
2 ==  3   calling double equals again, and start over
...
```
If not:
如果不是:

c. 检查是不是比较一方为boolean类型,如果是,将boolean转化为number类型,然后用==比较,重新开始.比如:
c. Check if we are comparing a boolean with something else. If we are, coerce the boolean to a number and call double equals and start over. Eg:
```
true == "3"
↓             coerce true to number 1.
1    == "3"   start over again.
...
```
If not:
如果不是

d. 检查是不是比较object和number,string或者symbol.如果是,将object转化为原始类型(怎么转化稍后说),再利用==进行比较,重新开始. 比如:
d. Check if we are comparing an object with a number, string, or a symbol. If we are, coerce the object to a primitive, call double equals on the result and start over again. Eg:
```
{ a: 'hello'}      ==     "5"
     ↓                          coerce the object to a primitive
"[object Object]"  ==     "5"   and start over again
...
```
Another example:
另一个例子:
```
[1,2,3]     ==     "5"
  ↓                       coerce object to a primitive
"1,2,3"     ==     "5"    and start over again
...
```
我将会在解释 object是如何转化为原始类型,现在这部分与我无关.
(I’ll explain how the objects get coerced to primitives in a second, but for now bare with me.)

如果不是, 我们就不再需要比较.返回false就行了.
If not, we don’t have anything else to check. If none of the steps above matches, return false at the end.

这里有张完整的照片.
Here is the complete picture:
![object转化为原始类型](https://cdn.filestackcontent.com/eTIpVrOQcqBNTN6Ma8i4)

在这里描述下图片内容.
enter image description here

你可以点击[这里](https://meyghani.com/projects/double-equals/)查看大图.
You can click here to see the larger version.

## 抽象的操作符
## Abstract Operators:
### 强制转化object为原始类型
### Coercing Objects to Primitives
我答应过会解释object如何转化为原始类型. 这是它如何工作的:
I promised to explain how objects get coerced to primitives. This is how it works:

**首先**默认情况下,会调用它的valueOf方法.如果返回原始类型,就用返回值.
**First** by default, call the valueOf method of the object. If valueOf returns a primitive, use that.

**否则**调用toString方法.如果返回原始类型,就用返回值.
**Otherwise** call the toString method of the object. If toString returns a primitive, use that

**否则**丢出一个错误.
**otherwise** throw an error.

**注意1**:这步是默认发生的.如果object和string比较,js会先调用toString,再调用valueOf.如果是和number比较,就先调用valueOf,再调用toString.
**Note 1**: These steps happen by default. If it is hinted to prefer string, JavaScript will call toString first, and then valueOf. By default JavaScript is hinted to prefer number, so it will call valueOf first, and then toString.

**注意2**:date类型的object也是同理.
**Note 2**: When coercing a Date object to a primitive, JavaScript is hinted to prefer string. So it will call toString first and then valueOf. In all the other cases, the default preferred type hint is number.

让我们看看相关的例子:
Let’s look at some examples:

示例1:
Example 1:
```
// coercing an empty object to a primitive
var x = {};
x.valueOf();   // -> {} : not a primitive, call toString now.
x.toString();  // -> "[object Object]": string is a primitive, I like it!
```
在上面例子中,当x转化为原始类型,结果会展示为"[object Object]".
In the example above, when x is coerced to a primitive, the result would be the funny looking string: "[object Object]"

示例2:
Example 2:
```
// coercing an object with custom `valueOf`
var x = {name: 'Tom'};
x.valueOf = function () {return 5;};
x.valueOf();   // -> 5 : number primitive, I liked it!
```
在上面例子中,当x转化为原始类型,结果返回5.
In the example above, when x is coerced to a primitive, the result would be the number 5.

示例3:
Example 3:
```
// coercing an array of numbers to a primitive
var x = [1,2,3];
x.valueOf();   // -> [1,2,3] : not a primitive, call toString now.
x.toString();  // -> "1,2,3" : string is a primitive, I like it!
```
上面例子中,当x转化为原始类型,结果返回"1,2,3".
In the example above, when x is coerced to a primitive, the result would be the string: "1,2,3"

示例4:
Example 4:
```
// coercing an array of elements with different types to a primitive
var x = [undefined, null, true, 1, "5", new Date(), {a: 2}, [1,2,3]]
x.valueOf();   // -> Array itself : not a primitive, call toString now.
x.toString();  // -> ",,true,1,5,Sat Mar 05 2016 10:59:47 GMT-0500 (EST),[object Object],1,2,3" : string is a primitive, I like it!
```
上面例子中,当x转化为原始类型,结果返回string:
In the example above, when x is coerced to a primitive, the result would be the string:<br>
`",,true,1,5,Sat Mar 05 2016 10:59:47 GMT-0500 (EST),[object Object],1,2,3"`

**强制转换成**number: ToNumber(输入值)
**Coercing to** number: ToNumber(input)

下面是不同输入值使用ToNumber后返回值的`总结`:
The following `summarizes` what ToNumber does for different input types:

* undefined → NaN
* null → 0
* number → return the number itself
* boolean → 0 if input is false, 1 if input is true
* string → parse string, if string is a number return number, otherwise return NaN. Empty string returns 0.

Eg: "5" → 5, "" → 0, "29xY" → NaN

* object: call ToPrimitive first, then call ToNumber on the result of ToPrimitive. See the toPrimitive section to learn how objects get coerced to primitives

* Date → calls getTime() method and returns the value

**强制转换成**string: ToString(输入值)
**Coercing to** string: ToString(input)

下面是不同输入值使用ToString后返回值的`总结`:
The following summarizes what ToString does for different input types:

* undefined → "undefined"
* null → "null"
* number → “number”
* boolean → “true”, “false”
* string → return the string itself.

* object: call ToPrimitive first with a hint of string, then call ToString on the result of ToPrimitive. See the toPrimitive section to learn how objects get coerced to primitives

* Date → calls toString() method and returns the value

**强制转换成**string: ToBoolean(输入值)
**Coercing to** boolean: ToBoolean(input)

在js中,以下值会转化为false:
In JavaScript, the following values are falsy:

* undefined
* null
* 0
* ""
* NaN

除此以为都为true.知道了这些事实以后就能很容易的知道被boolean转化后的值:
Everything else is truthy. Knowing this fact will make it easy to know what happens after a value is coerced to a boolean:

* undefined → false
* null → false
* number → false if 0, otherwise, true
* boolean → input iteself
* string → false if empty string "", otherwise true
* object → true. This follows that array → true
* Date → true

## 双等于的例子
## Double Equals Examples

现在我们知道==是如何工作的,我们可以看下一些有趣的例子.我会解释为什么js决定输出这些玩意儿.
Now that we know how double equals work, we can have some fun and look at some examples. I am going to explain what path JavaScript take to decide what to output.

Example: [] == 0

1. 类型不同？是的,我们必须绕个远路:
2. 是否比较为undefined和null,不是,继续检查:
3. 比较number和string? 不是,继续检查:
4. 比较一方有boolean? 不是,继续检查:
5. 比较object和number或string或symbol? 是的,我们比较array和number.好的,让我们将array转化成原始类型,然后再调用==进行比较.


1. Are types different? Yes, so we have to take the long path:
2. Are we comparing null with undefined? No, go to next check:
3. Are we comparing a number with a string? No, go to next check:
4. Are we comparing a boolean with something else? No, go to the next check:
5. Are we comparing an object with a number or string, or symbol? YES! we are comparing the array object with a number. Ok, let’s coerce the array to a primitive and call double equals again:
** calling valueOf on the array: output is [], which is not a primitive and I can’t use it. Let’s call toString:
** calling toString on the array: output is "". Perfect, it is a primitive string, and I can use it.

Calling double equals again with the result of coercing the object to a primitive:

`"" == 0`

And now we go back to the beginning and start over again:

1. Are the types the same? no, take the long path:
2. b. Are we comparing null with undefined? no, go to next check:
3. Are we comparing a number with a string? YES, now let’s coerce the string to a number. coercing the empty string to a number will return 0. Call double equals again and start over:

`0 == 0`

1. Are the types the same? Yes, call triple equals 0 === 0 and the result is `true`

So in summary:

```
[] == 0
↓
"" == 0
↓
0 == 0
↓
0 === 0
→ true
```

让我们记住这个例子,然后看看其他例子.在下面例子中,我将让你`完成` `整个` `算法`.
Let’s keep this example in mind and look at other examples. In the following examples, I will let you to `go through` the `whole` `algorithm`.

Example: [] == "0"
```
[] == "0"
↓
"" == "0"
↓
"" === "0"
→ false
```
Example: false == 1

```
false == 1
↓
0 == 1
↓
0 === 1
→ false
```
Example: {} == false

```
{} == false
↓
{} == 0
↓
"[object Object]" == 0
↓
NaN == 0
↓
NaN === 0
→ false
```
Example: undefined == false

```
undefined == false
↓
undefined == 0
→ false
```
Example: [1,2,3] == 123:

```
[1,2,3] == 123
↓
"1,2,3" == 123
↓
NaN    ==  123
↓
NaN   ===  123
↓
→ false
```
Example: [123] == 123:

```
[123] == 123
↓
"123" == 123
↓
123    ==  123
↓
123   ===  123
↓
→ true
```
Example: [123] == "123":

```
[123] == "123"
↓
"123" == "123"
↓
"123" === "123"
↓
→ true
```

在下面例子中,x被重新定义:
For the following examples, x is defined as the following:

```
var x = {
  a: '...',
  toString: function () {
    return false;
  },
  valueOf: function () {
    return new Boolean(true);
  }
};
```

Example: x == "0":

```
x == "0"
↓
false == "0"
↓
0    ==  "0"
↓
0    ==  0
↓
0   === 0
↓
→ true
```
Example: x == 5:

```
x == 5
↓
false == 5
↓
0    ==  5
↓
0   ===  5
→ false
```
Example: x == [1,2,3]:
```
x == [1,2,3] // both are objects
↓
x === [1,2,3] // they are stored at different addresses
→ false // the address is not the same
```
###Conclusion

So, which one should you choose? I’ll let you decide on that :)

I hope the == vs === JavaScript examples in this tutorial will guide you to code better.

###Other tutorials you might be interested in

JavaScript Best Practices: Tips & Tricks to Level Up Your Code
Beginner’s Guide: the Best Way to Learn JavaScript
Top Ten Things Beginners Must Know About JavaScript
Skills JavaScript Developers Should Learn in 2016
4 Easy Ways to Start Using ES2015