# 现代js框架存在的深层原因
[The deepest reason why modern JavaScript frameworks exist](https://medium.com/dailyjs/the-deepest-reason-why-modern-javascript-frameworks-exist-933b86ebc445)

我看到很多很多人`盲目的`使用类似于react,angular,vue的前端框架.这些框架提供很多有趣的东西,但是人们`忽略了他们存在的深层原因`,不是因为以下的原因:
I’ve seen many, many people using a modern framework like React, Angular or Vue.js `blindly`. These frameworks provide lots of interesting things, but usually people `miss the point about the deepest reason` of their existence, which is not:

他们由组件构成
They are based on components;
他们有很健壮的社区
They have a strong community;
他们有`大量的`第三方`插件`来处理事情
They have `plenty of` third party `libraries` to deal with things;
他们有很好用的第三方组件
They have useful third party components;
他们有`浏览器 扩展`来帮助debug东西
They have `browser extensions` that help debugging things;
他们对开发单页面应用很友好
They are good for doing single page applications.

`本质`、`根本`、`最深层的原因`是
The `essential`, `fundamental`, `deepest reason` is this one:

保持UI和状态`同步`是困难的
Keeping the UI in `sync` with the state is hard

是的,只有这个原因,然后让我们看下为什么
Yes, that’s it, and let’s see why
想象你`实现`一个用户可以通过`介绍`他的邮箱地址来`邀请`许多人的web应用.UX/UI设计者做了这样一个决定:这有一个展示帮助信息的label的空state,在其他`情况(state)`,我们展示邮箱地址,`在右边`有一个button/link去删除他们.
Imagine you are `implementing` a web application where the user can `invite` many people by `introducing` their email addresses. The UX/UI designer makes this decision: there is an empty state where we show a label with some help text, in any other `case` we show the email addresses with a button/link to delete them `to the right`.

表单的状态`基本上`是一个由邮件地址加一个唯一`标识符`组成的数组.`开始的时候`,这是空的.当通过`添加文字`和`敲击回车`来添加一个邮箱地址,你把`输入的邮箱地址`添加到数组中,并且更新了UI.当用户点击"delete",你删除邮箱地址并更新UI.你看到了吗?每当你改变state,你需要更新UI.
The state of this form can be `basically` an array of objects containing the email addresses plus a unique `identifier`. `Initially` it will be empty. When adding an email address by `typing it` and `hitting enter` you add `the typed email address` to the array and update the UI. When the user clicks on “delete” you delete the email address and update the UI. Have you seen that? Every time you change the state, you need to update the UI.

所以呢? 好的,看下我们如何在没有框架的帮助下`实现`上面的更新操作.
So what? Well, let’s see how we can `implement` it without the help of any framework:

一个关于`少量`的`复杂`UI的`普通``实现`
A `vanilla` `implementation` of a `somewhat` `complex` UI

run 看图

这代码很好`说明`用js实现一个小复杂的UI变化需要的`工作量`,(使用类似于jq之类的经典库,代码量也是类似的)
The code `illustrates` very well `the amount of work` needed to make a somewhat complex UI with vanilla JavaScript (using classic libraries such as jQuery would have been similar).

这个例子中,静态的`体系`在html上被创建,`反之` `动态的 素材`在js里被创建(使用document.createElement).这里有一个问题:构建UI的js代码可读性不是很好而且我们需要在两个地方`定义`UI.我们可以使用`innerHTML`,它的可读性会好很多,但是效率低,而且会造成`导致 跨 站 脚本`的漏洞.我们`也`可以使用`模板 引擎`,但是如果你`重新生成[再生]`一个大的`dom 树`,你会有两个问题: `效率不高`和你必须经常和事件处理重新建立连接.
In the example the static `structure` is created in the HTML, `whereas` the `dynamic` `stuff` is created in JavaScript (with document.createElement). This is the first problem: the JavaScript code that builds the UI is not very readable and we are `defining` the UI in two different parts. We could have used `innerHTML` and it could have been more readable but it’s `less efficient` and can `cause Cross Site Scripting` bugs. We could have used a `templating engine` `as well`, but if you `regenerate` a big `subtree of the DOM` you have two problems: is `not very efficient` and you have to usually reconnect the event handlers.

但是,这不是最严重的问题.最严重的问题是在每次改变都会更新UI.每当state改变,就会有大量的代码去更新UI.当在实例中添加一个邮箱地址,花费了两行代码区更新state,但是13行代码去更新UI.而且我们`尽可能的简单`来设计UI！
But that’s not the biggest problem. The biggest problem is always updating the UI on every state change. Every time the state is updated there is a lot of code required to update the UI. When adding an email address in the example it takes 2 lines of code to update the state, but 13 lines to update the UI. And we made the UI `as simple as possible`!!

看图

这不仅仅是难以书写和难以`推理`,而且很重要的一点是:这也`非常``脆弱`.设想我们需要实现一个从sever同步list的方法.我们将需要比较我们和服务端收到的data.这`包括`比较全部的id和内容(我们在`内存`中有一个不同数据相同id的记录副本)
It is not only hard to write and hard to `reason about`, but more importantly: it is also `extremely` `fragile`. Imagine we need to implement the ability to sync the list with a server. We will need to compare our data with the data received from the server. This `involves` comparing all the identifiers and the content (we could have in `memory` a copy of a record with the the same identifier but with different data). 

我们将需要实现大量`ad-hoc``呈现`代码有效`改变`DOM.如果我们产生了任何微小的错误,UI将会与数据不同步: 丢失信息,展示错误信息或者完全`搞乱 elements``对使用者没有做出相应`或`甚至 更加糟糕`,`触发`错误的操作(`例如` 点击一个删除按钮删除错误的item).
We will need to implement a lot of ad-hoc `presentation` code to `mutate` the DOM efficiently. And if we make any minimal mistake, the UI will be `out of sync from the data`: missing information, showing wrong information, or completely `messed up` with elements `not responding to the user` or `even worse`, `triggering` wrong actions (e.g. clicking on a delete button deletes the wrong item).

所以,保持UI同步数据`涉及`写大量的`繁琐的` `虚弱的` `不健壮的代码`
So, maintaining the UI in sync with the data `involves` writing a lot of `tedious`, `fragile`, and `fragile code`.

`公布的` UIS `营救`
`Declarative` UIs to the `rescue`
看图

所以,不是社区,不是工具,也不是`生态系统`,也不是第三方库等等.
So it is not the community, it is not the tools, nor the ecosystem, nor the third-party libraries,…
  `到目前为止`,这些框架最大 `改进`是提供 有能力`实现` `保证`应用`内部`状态同步 的 UIS.
  The biggest, `by far`, `improvement` these frameworks provide is having the ability to `implement` UIs that are `guaranteed` to be in sync with the `internal` state of the application

好的，`差不多了`. 这是正确的 如果你没有打乱一些每个`特别`框架也许有的规则(比如 状态的不可改变性).
Well, `almost`. It is true if you don’t mess with some rules that each `particular` framework might have (e.g. immutability of the state).

我们定义UI在单一的`开枪[位置]`,不需要在每个方法中写特定的UI代码,而且由于一个特定的状态，我们总是获取相同的`输出`:当状态改编后,框架自动更新.
We define the UI in a single `shot`, not having to write particular UI code in every action, and we always get the same `output` due to a particular state: the framework automatically updates it after the state changes.

它是如何工作的?
How does it work?

这有两个基础的`策略`
There are two basic `strategies`:

重新render整个组件: react. 当组件状态发生改变,他会在内存中render DOM,并且将他和已存在的DOM进行比较.事实上,过程很`昂贵的`，它render一个虚拟DOM然后比较提供的虚拟DOM的 `快照`[之前的虚拟DOM].接着它`计算`改变和`执行`相同的改变在真实的DOM.这个过程被叫做`reconciliation[和解]`.
Re-render the whole component: React. When the state of a component changes it renders a DOM in memory and compares it with the existing DOM. Actually since that would be very `expensive` it renders a Virtual DOM and compares it with the previous Virtual DOM `snapshot`. Then it `calculates` the changes and `performs` the same changes to the real DOM. This process is called reconciliation.

使用观察者观察变化：Angular和Vue.`观察`状态`变量`,当状态变量涉及到时,再将该部分DOM进行改变.
Watch for changes using observers: Angular and Vue.js. Your state `variables` are `observed` and when they change only the DOM elements where those values are/were involved in the rendering are updated.

web组件是怎么实现的?
What about web components?
很多时候,人们将web组件和react,angular,vue比较.这是一个清楚的`指示器`,很多人没有理解最大的`好处`这些框架提供的:保持UI状态的同步性.而且web组件在这方面没有提供任何东西.他们只是提供一个 template 标签,但是它没有提供任何的`一致``机能`.如果你想要使用web组件,并且想在app中使UI与`内部`状态同步,你必须手动完成,或者你可以使用一些类似于Stencil.js(它`内部`使用了虚拟dom,类似于React).
Many times people compare React, Angular and Vue.js with web components. This is a clear `indicator` that many people do not understand the biggest `benefit` these frameworks provide: keeping the UI in sync with the state. And web components doesn’t provide anything for that. They just provide a <template> tag but it doesn’t provide any `reconciliation` `mechanism`. If you want to use web components and have the UI in sync with the `internal` state of your app you have to do it by hand, or you can use something like Stencil.js (which `internal`ly uses a Virtual DOM, like React).

`让我们明确一点`: 关于科技最大的`潜力`不是组件:他总是保持UI和state的同步. Web组件`直接[开箱即用]`没有提供任何东西关于同步的东西,而且你必须使用第三方库来解决这个问题(或者手动解决).这是不可能的去写复杂有效而且`易于维护`的UI通过基础的js.这就是最主要的原因你需要一个现代js框架.
But `let’s make it clear`: it is not the components the great `potential` of these technologies: it is having always the UI in sync with the state. Web components doesn’t provide anything for it `out of the box`, and you have to use third party libraries to solve that (or do it by hand). It is not possible to write complex, efficient and `easy to maintain` UIs with Vanilla JavaScript. That’s the main reason you need a modern JavaScript framework.

自己动手干
Do it yourself
我喜欢学习事情的`原理`,而且`结果发现`这是虚拟dom`实现方式`在这之外.所以, 为什么我们不尝试使用一个不依靠任何已有框架的帮助的虚拟dom去重写我们vanilla UI.
I love to learn the fundamentals of things and `it turns out that` there are Virtual DOM `implementations` out there. So, why don’t we try to rewrite our vanilla UI implementation just using a Virtual DOM implementation without the help of any existing framework?

这是框架的`核心`,任何组件的基础类型.
Here’s the `core` of the framework, the base class for any component.
看图

而且这是 地址列表组件 `重新实现`.(通过一个babel的帮助来支持jsx)
And here’s the `reimplementation` of the AddressList component (with the help of a babel transform to support JSX):
看图

现在UI是`公开的`,而且我们不用使用任何框架.我们可以`实现`新的`逻辑`在任何方法改变状态,而且我们不必写`额外的`代码来保持UI的同步.问题解决！
Now the UI is `declarative` and we haven’t used any framework. We can `implement` new `logic` that changes the state in whatever way, and we don’t have to write `additional` code to keep the UI in sync. Problem solved!

现在除了对事件的处理,这看起来有点像react app,是吗? 我们有xxx 方法,一旦我们解决了 保持UI 和 `内部`state 同步的问题,`自然地`每件事情都`堆叠起来[好起来]`.
Now, except for the event handling, this looks like a lot like a React app, right? We haverender() ,componentDidMount() , setState() ,… Once you solve the problem of keeping the UI in sync with the `internal` state of the application, everything else `stacks up` `naturally`.

你可以在github项目中找到全部的源代码.
You can find the full source code in this Github repository.

结论
Conclusions
现代框架解决最主要的问题就是 UI和state同步的问题.
The main problem modern JavaScript frameworks solve is keeping the UI in sync with the state.
这不必使用Vanilla JS 去写一个 复杂的 高效的 简单的 易于维护的 UIS.
It is not possible to write complex, efficient and easy to maintain UIs with Vanilla JavaScript.
web组件没有为这个问题提供一个解决方式。
Web components do not provide a solution to this problem.
不在困难使用一个已存在的虚拟DOM库来构造你自己的框架.但是 我不支持你这么做！
It’s not that hard to make your own framework using an existing Virtual DOM library. But I’m not suggesting you to do that!




blindly 盲目地
sync 同步 async 异步

typing的形容词 typed ？？？

somewhat ---- a little

less efficient 效率低

out of sync from the data 与数据不同步
mess up 搞乱
e.g. 例如

improvement 改进
provide 提供

by far 到目前为止  --------- 长远来说???

since

technologies 科技 konwleage 知识？？？

it truns out that there are xxx out there. 这里有xxx