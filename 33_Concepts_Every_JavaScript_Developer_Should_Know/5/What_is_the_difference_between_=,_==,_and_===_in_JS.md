[原文](https://www.codecademy.com/en/forum_questions/558ea4f5e39efed371000508)

# What is the difference between =,==, and === in JS?
# 在js中, = == === 的区别
请通过相关例子说明在js中 = (`赋值`运算符), == 和 === (关系运算符)三者的区别.与相关的例子.而且我们通过例子来解释===中类型转换.为什么会出现下列现象?
The = (`assignment` operator) , == and === (relational operator) please explain the diffenerce between these 3 operators in javascript;`along with` `relevant` examples.And what do we mean by type type conversion in===,please explain with example.And why does

```js
3==='3'//false
    3==="3"//false
    "3"==3//true
    3===3//true
```

又为什么呢
also why does

```js
3==3//true
    "3"==3//true
    3=='3'//true
    1==true//true
```
通过使用=来给某些变量赋值
By using = you assign a value to something

```js
x = 1 //x now equals 1
x = 2 //x now equals 2
```
通过使用==来比较两者是否相等,这是不严谨的.
By using == you check if something is equal to something else. This is not strict

```js
x == 1 //is x equal to 1? (False)
x == 2 //is x equal to 2? (True)
true == 1 //does the boolean value of true equal 1? (True)
```

通过使用===来比较两者是否相等,这是严谨的.
By using === you check if something is equal to something else. This is also strict.

```js
x === 1 //is x equal to 1? (False)
x === 2 //is x equal to 2? (True)
true === 1 //does the boolean value of true equal 1? (False)
```
`防止` 它不清晰,严格的做法不仅仅检查两者值的相等,而且要比较两者的类型.
使用==将会将一边的类型转换成另一边的类型.比如bool和int类型.布尔值true就会变成1,所以1 等于1?是的.然而，当使用严格等于,他不会进行类型的转换.它会直接比较true和1,因为他们是两种不同的数据类型,所以会返回 false.
What strict does, in case it wasn't clear there, is that it checks not only the equality of the two values, it compares the types of the two values too. Using == will try and convert one side of the expression to be the same type as the other. For example, boolean and integer. The boolean value for true is 1, `therefore` does 1 equal 1? Yes, true. When using strict however, it does not try and convert before doing the comparison, it checks if true equals 1, which is doesn't as they are two different data types, and returns false.

Hope this helps!