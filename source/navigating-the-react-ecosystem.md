title: React.js生态系统概览 [译]
date: 2015-10-18 10:00:00
update: 2015-10-27 10:00:00
author: me
tags:
    - 前端
    - 开发
cover: -/images/react-ecosystem/1.jpg

---

JavaScript领域发展速度很快，甚至有人认为这已经引起了[负效应](http://www.breck-mckye.com/blog/2014/12/the-state-of-javascript-in-2015/)。一个前端库从早期开发的小玩具，到流行，再到过时，可能也就几个月时间。判断一个工具能否在几年内依然保持活力都快成了一门艺术了。

React.js在两年前[发布](https://facebook.github.io/react/blog/2013/06/05/why-react.html)时，我刚开始学Angular，React在我看来只是又一个模板库而已。这两年间，Angular得到了[JavaScript开发者](http://www.toptal.com/javascript)的认同，它几乎成了现代前端开发的代名词。我还看到一些很保守的团队都在用它，这让我觉得Angular好像就是未来。

但突然发生了件奇怪的事，Angular好像[成了奥斯本效应的受害者](http://jaxenter.com/angular-2-0-announcement-backfires-112127.html)，或者说它被提前宣布了死亡。Angular团队宣布，Angular 2将会完全不同，基本没有从Angular 1升级迁移的东西，而且Angular 2在接下来的一年里还用不了。这告诉了那些想开发新Web项目的人：你想用一个马上要被淘汰了的框架写项目吗？

开发者们的忧虑影响到了正在建立的React社区，但React总标榜它只是MVC中的视图层(V)，让一些依赖完整MVC框架做开发的人感觉有点失望。如何补充其他部分的功能？自己写吗？还是用别的三方库？要是的话该选哪一个呢？

果然，Facebook(React.js的创始)出了另一个杀手锏：[Flux](https://facebook.github.io/react/blog/2014/05/06/flux.html)工作流，它声称要填补模型层(M)和控制层(C )的功能。Facebook还称Flux只是一种“模式”，不是个框架，他们的Flux实现只是这个模式的一个例子。就像他们所说，这个实现过于简单，但还是要写很多代码和一堆重复的模板才跑得起来。

这时开源社区发力了，一年后便有了各种Flux实现库，甚至都出来[比较他们](https://github.com/voronianski/flux-comparison)的元项目了。Facebook激起了社区的兴趣，不是给出现成的东西，而是鼓励大家提出自己的解决方案，这点很不错。

当你要结合各种库开发一个完整架构时，摆脱了框架的束缚，独立的库还可以在很多地方重用，在自己构建架构的过程中，这个优点很明显。

这便是为什么React相关的东西这么有意思。它们可以很容易地在其他JavaScript环境中实现重用。就算你不打算用React，看看它的生态系统都能受到启发。可以试试强大又容易配置的模块化打包工具[Webpack](http://webpack.github.io/)来简化构建系统，或者用[Babel](https://babeljs.io/)转译器马上开始用ECMAScript 6甚至ECMAScript 7来写代码。

在这篇文章里我会给你概览一遍这些有意思的库和特性，来探索下React整个生态系统吧。

![](-/images/react-ecosystem/1.jpg)

## 构建系统

创建一个新的Web项目时，首先要考虑的可能就是构建系统了。它不只是做为个运行脚本的工具，还能优化你的项目结构。一个构建系统必须能包括下面几个最主要功能：

- 管理内部与外部依赖
- 运行编译器和预处理器（例如CoffeeScript与SASS）
- 为生产环境优化资源（例如Uglify）
- 运行开发环境的Web Server，文件监控，浏览器自动刷新

最近几年，[Yeoman](http://yeoman.io/learning/)，[Bower](http://bower.io/)与[Grunt](http://gruntjs.com/)被誉为现代前端开发的三剑客。他们解决了生成基础模板，包管理和各种通用任务问题，后面也很多人从Grunt换到了[Gulp](http://gulpjs.com/)。

在React的生态系统里，基本上可以丢掉这些东西了，不是说你用不到他们，而是说可以用更先进的[Webpack](http://webpack.github.io/)与[NPM](https://www.npmjs.com/)。怎么做到的呢？Webpack是一个模块化打包工具，用它可以在浏览器环境下使用Node.js中常用的[CommonJS](http://en.wikipedia.org/wiki/CommonJS)模块语法。其实它要更简单点，因为你不用为了前端另外学一种包管理方案。只需要用NPM，就可以做到服务端与前端模块的共用。也不用处理JS文件按顺序加载的问题，因为它能够从每个文件的import语法中推测出依赖关系，整个串联成一个可以在浏览器中加载的脚本。

![Webpack](-/images/react-ecosystem/2.jpg)

更强大的是Webpack不只像同类工具[Browserify](http://browserify.org/)，它还可以处理其他类型的资源。例如用[加载器](http://webpack.github.io/docs/loaders.html)，可以将任何资源文件转换成JavaScript函数，去内联或加载引用到的文件。用不着手工预处理还有从HTML中引用资源了，只要在JavaScript中`require`CSS/SASS/LESS文件就好了，Webpack会根据[配置文件](https://github.com/webpack/example-app/blob/master/webpack.config.js)的描述去搞定一切。它还提供了个开发环境的Web Server，文件监控器，你可以在`package.json`中使用`scripts`域去定义一个任务：

``` json
{
  "name": "react-example-filmdb",
  "version": "0.0.1",
  "description": "Isomorphic React + Flux film database example",
  "main": "server/index.js",
  "scripts": {
    "build": "./node_modules/.bin/webpack --progress --stats --config ./webpack/prod.config.js",
    "dev": "node --harmony ./webpack/dev-server.js",
    "prod": "NODE_ENV=production node server/index.js",
    "test": "./node_modules/.bin/karma start --single-run",
    "postinstall": "npm run build"
  }
  ...
}
```
这些东西就可以代替Gulp与Bower了。当然，还是可以继续用Yeoman去生成应用基础模板的。要是Yeoman生成不了你需要的东西时（其实大多时候也都需要删掉那些用不到的库），还可以从Github上Clone一个基础模板，然后再改改。

## 马上试试新的ECMAScript

JavaScript在这几年有很大改善，移除糟粕稳定语言后，我们看到了很多新特性，ECMAScript 6(ES6)草案已经[定稿](http://wiki.ecmascript.org/doku.php?id=harmony:specification_drafts#final_draft)。ECMAScript 7也已纳入标准化日程中，它们的特性也都已经被很多库采用了。

![ECMAScript 7](-/images/react-ecosystem/3.jpg)

可能你觉得到IE支持之前都用不上这些JS新特性，但实际我们不需要等浏览器完全支持，ES转译器已经广泛应用了。目前最好的ES转译器是[Babel](https://babeljs.io/)，它能够把ES6+代码转换成ES5，所以你马上就能用上新ES特性了（指已经在Babel中实现的那些，其实一般新出的特性也会很快被支持）。

![Babel](-/images/react-ecosystem/4.jpg)

新的JavaScript特性在所有前端框架里都可以用，更新的React能很好的在ES6与ES7下运行。这些新特性可以解决在用React开发时遇到的一些问题。来看下这些改善吧，它们对React项目很有用。稍后我们再看看怎样利用这些语法，来搭配使用React的工具和库。

### ES6 Classes

面向对象编程是一种强大又广泛适用的范式，但在JavaScript里感觉有点不一样。Backbone，Ember，Angular，或React等大多数前端框架都有它们自己的定义类和创建对象的方式。在ES6中，有原生的类支持了，它简洁清晰而不用我们自己实现，例如：

``` javascript
React.createClass({
  displayName: 'HelloMessage',
  render() {
    return <div>Hello {this.props.name}</div>;
  }
})
```

使用ES6就可以写成：

``` javascript
class HelloMessage extends React.Component {
  render() {
    return <div>Hello {this.props.name}</div>;
  }
}
```

再看个详细的例子：

``` javascript
React.createClass({
  displayName: 'Counter',
  getDefaultProps: function(){
    return {initialCount: 0};
  },
  getInitialState: function() {
    return {count: this.props.initialCount}
  },
  propTypes: {initialCount: React.PropTypes.number},
  tick() {
    this.setState({count: this.state.count + 1});
  },
  render() {
    return (
      <div onClick={this.tick}>
        Clicks: {this.state.count}
      </div>
    );
  }
});
```

ES6可以写成：

``` javascript
class Counter extends React.Component {
  static propTypes = {initialCount: React.PropTypes.number};
  static defaultProps = {initialCount: 0};

  constructor(props) {
    super(props);
    this.state = {count: props.initialCount};
  }

  state = {count: this.props.initialCount};
  tick() {
    this.setState({count: this.state.count + 1});
  }

  render() {
    return (
      <div onClick={this.tick.bind(this)}>
        Clicks: {this.state.count}
      </div>
    );
  }
}
```

在这里不用再写`getDefaultProps`和`getInitialState`这两个React的生命周期函数了。`getDefaultProps`改为了类的静态变量`defaultProps`，初始state也只需要定义在构造函数中。这种方式唯一的缺点是，在[JSX](https://facebook.github.io/react/docs/jsx-in-depth.html)中使用的方法的上下文不会再自动绑定到类实例了，必须用`bind`来[指定](https://facebook.github.io/react/blog/2015/01/27/react-v0.13.0-beta-1.html)。

## 装饰器

装饰器是ES7中的特性。通过一个包装函数，来增强函数或类的行为。例如想为一些组件使用同一个change handler，
而又不想[inheritance antipattern](http://asserttrue.blogspot.cz/2009/02/inheritance-as-antipattern.html)，则可以用类的装饰器去实现。定义一个装饰器：

``` javascript
addChangeHandler: function(target) {
  target.prototype.changeHandler = function(key, attr, event) {
    var state = {};
    state[key] = this.state[key] || {};
    state[key][attr] = event.currentTarget.value;
    this.setState(state);
  };
  return target;
}
```

在这里，函数`addChangeHandler`添加了`changeHandler`方法到目标类`target`的实例上。

要应用装饰器，只需要：

``` javascript
MyClass = addChangeHandler(MyClass)
```

或者用更优雅的ES7写法：

``` javascript
@addChangeHandler
class MyClass {
  ...
}
```

因为React没有双向数据绑定，在用到input之类的控件时，代码就会比较冗余，`changeHandler`函数则把它简化了。第一个参数表示在state对象中使用的`key`，它将存储input的一个数据对象。第二个参数是属性，它表示input的值。这两个参数在JSX中被传入:

``` javascript
@addChangeHandler
class LoginInput extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      login: {}
    };
  }
  render() {
    return (
      <input
        type='text'
        value={this.state.login.username}
        onChange={this.changeHandler.bind(this, 'login', 'username')} />
      <input
        type='password'
        value={this.state.login.password}
        onChange={this.changeHandler.bind(this, 'login', 'password')} />
    )
  }
}
```

用户名输入框发生改变时，输入框的值会被直接存到`this.state.login.username`中，不需要一个个去写handler了。

## 箭头函数

JavaScript的动态上下文`this`不太直观，成了开发者的常痛了。比如在类里，包装函数中的`this`都被指向了全局变量。要fix这个问题，通常是把`this`存到外部作用域下(例如_this)，然后在内部函数中用它：

``` javascript
class DirectorsStore {
  onFetch(directors) {
    var _this = this;
    this.directorsHash = {};
    directors.forEach(function(x){
      _this.directorsHash[x._id] = x;
    })
  }
}
```

在ES6中，函数`function(x) {`可以写成`(x) => {`。这种箭头方式的函数定义不仅将内部的`this`绑定到了外部作用域，而且看起来很简洁，在写大量异步代码时很有用：

``` javascript
onFetch(directors) {
  this.directorsHash = {};
  directors.forEach((x) => {
    this.directorsHash[x._id] = x;
  })
}
```

## 解构赋值

ES6中的解构赋值允许在赋值表达式左边写个复合对象：

``` javascript
var o = {p: 42, q: true};
var {p, q} = o;

console.log(p); // 42
console.log(q); // true
```

在React中有什么实际用处吗？看下面这个例子：

``` javascript
function makeRequest(url, method, params) {
  var config = {
    url: url,
    method: method,
    params: params
  };
  ...
}
```

用解构方式，可以一次给几个键赋值。`url, method, params`这些值会被自动赋给有着同名键的对象中。这让代码更不易出错：

``` javascript
function makeRequest(url, method, params) {
  var config = {url, method, params};
  ...
}
```

解构赋值也可以用来加载一个模块的子集：

``` javascript
const {clone, assign} = require('lodash');
function output(data, optional) {
  var payload = clone(data);
  assign(payload, optional);
}
```

## 函数的默认，剩余，扩展参数

在ES6中函数传参更强大了，可以为函数设置默认参数：

``` javascript
function http(endpoint, method='GET') {
  console.log(method)
  ...
}

http('/api') // GET
```

觉得`arguments`用起来很麻烦？可以把剩余参数写成一个数组：

``` javascript
function networkAction(context, method, ...rest) {
  // rest is an array
  return method.apply(context, rest);
}
```

要是不想调用`apply()`方法，可以把数组扩展成函数的参数：

``` javascript
myArguments = ['foo', 'bar', 123];
myFunction(...myArguments);
```

## Generator与Async函数

Generator是一种可以暂停执行，保存状态并稍后恢复执行的函数，每次遇到`yield`关键字时，就会暂停执行。Generator写法：

``` javascript
function* sequence(from, to) {
  console.log('Ready!');
  while(from <= to) {
    yield from++;
  }
}
```

调用Generator函数：

``` javascript
> var cursor = sequence(1,3)
Ready!
> cursor.next()
{ value: 1, done: false }
> cursor.next()
{ value: 2, done: false }
> cursor.next()
{ value: 3, done: false }
> cursor.next()
{ value: undefined, done: true }
```

当调用Generator函数时，会立即执行到第一次遇到的`yield`关键字处暂停。在调用`next()`函数后，返回yield后的表达式的值(value)，然后Generator里再继续执行之后的代码。每次遇到`yield`都返回一个值，在第三次调用`next()`后，Generator函数终止，最后调用的`next()`将返回`{ value: undefined, done: true }`。

当然咯，Generator不只能用来创建数字序列。它能够暂停和恢复函数的执行，这便可以不需要回调函数就完成异步流的控制。

我们用异步函数来证明这一点。一般我们会做些I/O操作，这里为了简单起见，用`setTimeout`模拟。这个异步函数立即返回一个[promise](http://www.toptal.com/javascript/javascript-promises)。（ES6有[原生的promise](http://www.html5rocks.com/en/tutorials/es6/promises/)）：

``` javascript
function asyncDouble(x) {
  var deferred = Promise.defer();
  setTimeout(function(){
    deferred.resolve(x*2);
  }, 1000);
  return deferred.promise;
}
```

然后写一个消费者函数：

``` javascript
function consumer(generator){
  var cursor = generator();
  var value;
  function loop() {
    var data = cursor.next(value);
    if (data.done) {
      return;
    } else {
      data.value.then(x => {
        value = x;
        loop();
      })
    }
  }
  loop();
}
```

这个函数接受Generator函数作为参数，只要`yield`有值，就会继续调用`next()`方法。这个例子中，`yield`的值是promise，所以必须等待promise调用resolve后，再递归调用`loop()`循环。

resolve会调用`then()`方法，把resolve的结果赋值给`value`，value被定义在函数外部，它会被传给下一个`next(value)`。这次next的调用为`yield`产生了一个返回值(即value)。这样可以不用回调函数来写异步调用了：

``` javascript
function* myGenerator(){
  const data1 = yield asyncDouble(1);
  console.log(`Double 1 = ${data1}`);
  const data2 = yield asyncDouble(2);
  console.log(`Double 2 = ${data2}`);
  const data3 = yield asyncDouble(3);
  console.log(`Double 3 = ${data3}`);
}

consumer(myGenerator);
```

Generator函数`myGenerator`将在每次遇到`yield`时暂停，等待消费者函数的promise去resolve。在控制台每隔1秒的输出：

``` javascript
Double 1 = 2
Double 2 = 4
Double 3 = 6
```

上面的示例代码不推荐在生产环境中用，可以用更完善的[co](https://github.com/tj/co)库，能很简单地用yield处理异步，还包含了错误处理：

``` javascript
co(function *(){
  var a = yield Promise.resolve(1);
  console.log(a);
  var b = yield Promise.resolve(2);
  console.log(b);
  var c = yield Promise.resolve(3);
  console.log(c);
}).catch(function(err){
  console.error(err.stack);  
});
```

在ES7中，将异步处理的改进更近了一步，增加了`async`和`await`关键字，无需使用Generator。上面的例子可以写成：

``` javascript
async function (){
  try {
    var a = await Promise.resolve(1);
    console.log(a);
    var b = await Promise.resolve(2);
    console.log(b);
    var c = await Promise.resolve(3);
    console.log(c);
  } catch (err) {
    console.error(err.stack);  
  }
};
```

有了这些特性，不会觉得JavaScript写异步代码麻烦了，而且在任何地方都可以用。

Generator不仅简洁明了，还能做些用回调很难实现的事。比如Node.js中[koa](http://koajs.com/)Web框架的中间件。koa的目标是替换掉Express，它有个很不错的特性：中间件的上下游都可以对服务端响应做进一步修改。看看这段koa服务器代码：

``` javascript
// Response time logger middleware
app.use(function *(next){
  // Downstream
  var start = new Date;
  yield next;
  // Upstream
  this.body += ' World';
  var ms = new Date - start;
  console.log('%s %s - %s', this.method, this.url, ms);
});

// Response handler
app.use(function *(){
  this.body = 'Hello';
});

app.listen(3000);  
```

![Koa](-/images/react-ecosystem/5.jpg)

进入第一个中间件时(上游)，遇到yield后，先进入第二个中间件(下游)将响应体设置为`Hello`，然后再从yield恢复执行，继续执行上游中间件代码，为响应体追加了` World`，然后打印了时间日志，在这里上下游共享了相同的上下文(this)。这要比[Express](http://expressjs.com/guide/routing.html)更强大，如果用Express来写的话可能会是这样：

```javascript
var start;
// Downstream middleware
app.use(function(req, res, next) {
  start = new Date;
  next();
  // Already returned, cannot continue here
});

// Response
app.use(function (req, res, next){
  res.send('Hello World')
  next();
});

// Upstream middleware
app.use(function(req, res, next) {
  // res already sent, cannot modify
  var ms = new Date - start;
  console.log('%s %s - %s', this.method, this.url, ms);
  next();
});

app.listen(3000);
```

也许你已经发现问题了，使用全局的`start`变量在出现并发请求时会产生污染。

用koa时，可以很容易地用Generator做异步操作：

``` javascript
app.use(function *(){
  try {
    const part1 = yield fs.readFile(this.request.query.file1, 'utf8');
    const part2 = yield fs.readFile(this.request.query.file2, 'utf8');
    this.body = part1 + part2;
  } catch (err) {
    this.status = 404
    this.body = err;
  }
});

app.listen(3000);
```

可以设想下这个例子在Express中用promise和回调会是什么样子。

![](-/images/react-ecosystem/6.jpg)

上面讨论的Node.js这些跟React有关系吗？当然咯，Node是React的首选后端服务器。因为Node也用JavaScript，这样可以前后端共享代码，用来构建[同构](http://nerds.airbnb.com/isomorphic-javascript-future-web-apps/)的React Web应用，稍后会介绍到。

## Flux库

React很擅长创建视图组件，但我们还需要一种方式去管理数据和整个应用的状态。Flux应用架构被普遍认为是React最好的补充。要是你对Flux还很陌生，建议看看这篇快速[导览](https://facebook.github.io/flux/docs/overview.html)。

![Flux](-/images/react-ecosystem/7.jpg)

目前还没有出现大多数人都认同的Flux实现，Facebook官方的[Flux](https://github.com/facebook/flux)实现很多人都觉得很冗余。我们主要关心能不能少写点模板代码，配置方便，并且给多层组件提供些好用的功能，支持服务端渲染等等这些。可以看看[这里](https://github.com/kriasoft/react-starter-kit/issues/22)给这些实现库列出的一些指标。我关注了Alt, Reflux, Flummox, Fluxxor, 和Marty.js。

我选择库的方式并不能说客观，但很有用。[Fluxxor](http://fluxxor.com/)是我发现的第一个库，但现在看起来它有点旧了。[Marty.js](http://martyjs.org/)很有意思，有很多功能，但还是需要写很多模板代码，有些功能看起来比较多余。[Reflux](https://github.com/spoike/refluxjs)看起来蛮有吸引力，但感觉对初学者来说有点难，还缺少合适的文档。[Flummox](http://acdlite.github.io/flummox)和[Alt](http://alt.js.org/)很相似，但Alt不需要写很多模板代码，开发也非常活跃，[文档](http://alt.js.org/docs/)更新块，而且有个[Slack](http://www.reactiflux.com/)社区。所以我选了Alt。

## Alt Flux

![Alt Flux](-/images/react-ecosystem/8.jpg)

Alt的Flux实现简单而强大。Facebook的Flux[文档](https://facebook.github.io/flux/docs/overview.html)描述了很多关于dispatcher的东西，但在[Alt](https://github.com/goatslacker/alt)中这些都可以忽略，因为dispatcher被隐式关联到了action，通常不需要写多余代码。只需要关心store，action，component。这三层对应MVC模型：store为模型层，action为控6制层，component为视图层。差异是Flux模型是单向数据流，意味着控制层不能直接修改视图层，而是触发模型层后，视图层被动修改。这对[有些](http://joelhooks.com/blog/2013/04/24/modeling-data-and-state-in-your-angularjs-application/)Angular开发者来说已经是最佳实践了。

1. component初始化action
2. store监听action然后更新数据
3. component被绑定到store，当数据更新时就重新渲染

![Alt Flux](-/images/react-ecosystem/9.jpg)

## Action

使用Alt库时，action通常有两种写法：自动与手动。自动action用`generateActions`函数创建，它们直接发给dispatcher，手动方法则写成action类的方法，它们可以附带一个payload发给dispatcher。常用例子是自动action通知store有关app的一些事件。其余由手动action负责，它是处理服务端交互的首选方式。

REST API调用这种就属于action，完整流程如下：

1. Component触发action
2. action创建者发起一个异步服务器请求，把结果作为一个payload发给dispatcher
3. store监听action，对应的action处理器接收结果，然后store更新它的相应状态

对于AJAX请求，我们可以用[axios](https://github.com/mzabriskie/axios)库，无缝地处理JSON数据和头数据。可以用ES7的`async/await`关键字，免去promise与回调。如果`POST`响应状态不是2XX，抛出错误，然后发出返回的数据或接收到的错误。

看个简单的Alt登录示例，在这里注销action不需要做任何事情，只需要通知store，所以可以自动生成它。登录action是手动的，会把登录数据作为一个参数发给action的创建者。从服务端获取响应后，发出数据，有错误的话就发出错误。

``` javascript
class LoginActions {
  constructor() {
    // Automatic action
    this.generateActions('logout');
  }

  // Manual action
  async login(data) {
    try {
      const response = await axios.post('/auth/login', data);
      this.dispatch({ok: true, user: response.data});
    } catch (err) {
      console.error(err);
      this.dispatch({ok: false, error: err.data});
    }
  }
}

module.exports = (alt.createActions(LoginActions));
```

## Store

Flux模型的store有两个任务：作为action的处理器，保存状态。继续看看登录例子是怎么做的吧。

创建一个`LoginStore`，有两个状态属性：`user`用于当前登录的用户，`error`用于当前登录相关的错误。为了减少模板代码数量，Alt可以用一个`bindActions`函数绑定所有action到store类。

``` javascript
class LoginStore {
  constructor() {
    this.bindActions(LoginActions);
    this.user = null;
    this.error = null;
  }
  ...
```

处理器定义起来很方便，只需要在对应action名字前加`on`。因此`login`的action由`onLogin`处理。注意action名的首字母是陀峰命名法的大写。在`LoginStore`中，有以下两个处理器，被对应的action调用：

``` javascript
...
  onLogin(data) {
    if (data.ok) {
      this.user = data.user;
      this.error = null;
      router.transitionTo('home');
    } else {
      this.user = null;
      this.error = data.error
    }
  }

  onLogout() {
    this.user = null;
    this.error = null;
  }
}
```

## Component

通常将store绑定到component的方式是用mixin。但mixin快过时了，需要找个其他方式，有个新方法是使用嵌套的component。将我们的component包装到另一个component里面，它会托管store的监听然后重新调用渲染。component会在`props`中接收到store的状态。这个方法对于[smart and dumb](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0) component的代码组织很有用，是以后的趋势。对于Alt来说，包装component是由AltContainer实现的：

``` javascript
export default class Login extends React.Component {
  render() {
    return (
      <AltContainer stores={{LoginStore: LoginStore}}>
        <LoginPage/>
      </AltContainer>
  )}
}
```

`LoginPage` component也用了我们之前介绍过的`changeHandler`装饰器。`LoginStore`的数据用来显示登陆失败后的错误，重新渲染则由`AltContainer`负责。点击登录按钮后执行`login`action，完整的Alt工作流：

``` javascript
@changeHandler
export default class LoginPage extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      loginForm: {}
    };
  }
  login() {
    LoginActions.login(this.state.loginForm)
  }
  render() {
    return (
      <Alert>{{this.props.LoginStore.error}}</Alert>
      <Input
        label='Username'
        type='text'
        value={this.state.login.username}
        onChange={this.changeHandler.bind(this, 'loginForm', 'username')} />
      <Input
        label='Password'
        type='password'
        value={this.state.login.password}
        onChange={this.changeHandler.bind(this, 'loginForm', 'password')} />
      <Button onClick={this.login.bind(this)}>Login</Button>
  )}
}
```

## 同构渲染

同构Web应用是近些年来的热点话题，它能解决传统单页面应用最大的问题：浏览器用JavaScript动态创建HTML，如果浏览器禁用了JavaScript，内容就无法显示了，搜索引擎索引不到Web页面，内容不会出现在搜索结果中。实际上有方法[解决](http://lawsonry.com/2014/05/diy-angularjs-seo-with-phantomjs-the-easy-way/)这个问题，但做的并不够好。同构的方式是通过服务端渲染内容来解决问题。Node.js是服务端的JavaScript，React当然也能运行在服务端。

一些使用单例方式的Flux库，很难做服务端渲染，单例的Flux 在遇到并发请求时，store数据就会变得混乱。一些Flux库用实例来解决这个问题，但需要在代码间传递实例。Alt实际也提供了Flux实例，但它用单例解决服务端渲染问题，它会在每次请求后清空store，以便并发请求时每次store都是初始状态。

`React.renderToString`函数是服务端渲染的核心，整个React应用运行在服务端。服务端生成HTML后响应给浏览器。浏览器JavaScript运行时，再渲染剩余部分。可以用[Iso](https://github.com/goatslacker/iso)库实现这点，它同时也被集成到了Alt中。

首先，我们在服务端用`alt.bootstrap`初始化Flux，这会初始化Flux store数据。然后区分哪个URL对应的渲染哪个component，哪个由客户端`Router`渲染。由于使用`alt`的单例版，在每次渲染完后，需要使用`alt.flush()`初始化store以便为下次请求做好准备。使用`iso`插件，可以把Flux的状态数据序列化到HTML字符串中，以便客户端知道去哪找到这份状态数据：

``` javascript
// We use react-router to run the URL that is provided in routes.jsx
var getHandler = function(routes, url) {
  var deferred = Promise.defer();
  Router.run(routes, url, function (Handler) {
    deferred.resolve(Handler);
  });
  return deferred.promise;
};

app.use(function *(next) {
  yield next;
  // We seed our stores with data
  alt.bootstrap(JSON.stringify(this.locals.data || {}));
  var iso = new Iso();
  const handler = yield getHandler(reactRoutes, this.request.url);
  const node = React.renderToString(React.createElement(handler));
  iso.add(node, alt.flush());
  this.render('layout', {html: iso.render()});
});
```

在客户端，拿到了服务端的状态数据，用来初始化`alt`的状态。然后可以运行`Router`和`React.render`去更新服务端生成的HTML：

``` javascript
Iso.bootstrap(function (state, _, container) {
  // Bootstrap the state from the server
  alt.bootstrap(state)
  Router.run(routes, Router.HistoryLocation, function (Handler, req) {
    let node = React.createElement(Handler)
    React.render(node, container)
  })
})
```

## 一些有用的库

例如CSS布局容器，样式表单，按钮，验证，日期选择器等等。

React-Bootstrap

![](-/images/react-ecosystem/10.jpg)

Twitter的Bootstrap框架应用已经非常普遍，对那些不想花时间写CSS的开发者很有用。特别是在原型开发阶段，Bootstrap不可或缺。要在React中使用Bootatrap，可以试试React-Bootstrap，它使用原生React component重新实现了Bootstrap的jQuery插件。代码简洁易懂：

``` javascript
<Navbar brand='React-Bootstrap'>
  <Nav>
    <NavItem eventKey={1} href='#'>Link</NavItem>
    <NavItem eventKey={2} href='#'>Link</NavItem>
    <DropdownButton eventKey={3} title='Dropdown'>
      <MenuItem eventKey='1'>Action</MenuItem>
      <MenuItem eventKey='2'>Another action</MenuItem>
      <MenuItem eventKey='3'>Something else here</MenuItem>
      <MenuItem divider />
      <MenuItem eventKey='4'>Separated link</MenuItem>
    </DropdownButton>
  </Nav>
</Navbar>
```

个人感觉这才是简单易懂的HTML吧。

也可以在Webpack中使用Bootstrap，看你喜欢哪个CSS预处理器。支持自定义引入的Bootstrap组件，可以在CSS代码中使用LESS或SASS的全局变量。

## React Router

![](-/images/react-ecosystem/11.jpg)

[React Router](https://github.com/rackt/react-router)几乎已经成了React的路由标准了。它支持嵌套路由，重定向，同构渲染。基于JSX的例子：

``` javascript
<Router history={new BrowserHistory}>
  <Route path="/" component={App}>
    <Route path="about" name="about" component={About}/>
    <Route path="users" name="users"  component={Users} indexComponent={RecentUsers}>
      <Route path="/user/:userId" name="user" component={User}/>
    </Route>
    <Route path="*" component={NoMatch}/>
  </Route>
</Router>
```

React Router可以用`Link` component做应用导航，只需要指定路由名称：

``` javascript
<nav>
  <Link to="about">About</Link>
  <Link to="users">Users</Link>
</nav>
```

还有集成了React-Bootstrap的路由库，不想手写Bootstrap的`active`类，可以用[react-router-bootstrap](https://github.com/react-bootstrap/react-router-bootstrap)：

``` javascript
<Nav>
  <NavItemLink to="about">About</NavItemLink>
  <NavItemLink to="users">Users</NavItemLink>
</Nav>
```

## Formsy-React

表单开发通常都比较麻烦，用[formsy-react](https://github.com/christianalfoni/formsy-react)来简化一下吧，它可以用来管理验证和数据模型。Formsy-React不包含实际的表单输入，而是推荐用户自己写(这是正确的)。如果只用通用表单，可以用[formsy-react-components](formsy-react-components)。它包括了Bootstrap类：

``` javascript
import Formsy from 'formsy-react';
import {Input} from 'formsy-react-components';
export default class FormsyForm extends React.Component {
  enableButton() {
    this.setState({canSubmit: true});
  }
  disableButton() {
    this.setState({canSubmit: true});
  }
  submit(model) {
    FormActions.saveEmail(model.email);
  }
  render() {
    return (
      <Formsy.Form onValidSubmit={this.submit} onValid={this.enableButton} onInvalid={this.disableButton}>
        <Input
          name="email"
          validations="isEmail"
          validationError="This is not a valid email"
          required/>
        <button type="submit" disabled={!this.state.canSubmit}>Submit</button>
      </Formsy.Form>
  )}
}
```

## 日期与选择器

日期与选择器组件算是UI库的锦上添花了，遗憾的是这两个组件在Bootstrap 3上被移除了，因为它们对于一个通用CSS框架来说比较特殊了。不过我发现了两个不错的代替：[react-pikaday](https://github.com/thomasboyt/react-pikaday)和[react-select](https://github.com/JedWatson/react-select)。我尝试过10多个库，这两个总体来说很不错。非常易用：

``` javascript
import Pikaday from 'react-pikaday';
import Select from 'react-select';

export default class CalendarAndTypeahead extends React.Component {
  constructor(props){
    super(props);
    this.options = [
      { value: 'one', label: 'One' },
      { value: 'two', label: 'Two' }
    ];
  }
  dateChange(date) {
    this.setState({date: date});
  },
  selectChange(selected) {
    this.setState({selected: selected});
  },
  render() {
    return (
      <Pikaday
        value={this.state.date}
        onChange={this.dateChange} />
      <Select
        name="form-field-name"
        value={this.state.selected}
        options={this.options}
        onChange={selectChange} />
  )}
}
```

## 总结 - React.js

这篇文章介绍了目前Web开发的一些技术与框架。有些是React相关的，因为React的开放性，它们也能被用在其他环境。有时候技术进步总会被对新事物的恐惧所阻碍，我希望这篇文章可以打消你对尝试React, Flux和新的ECMAScript的疑虑。

![](-/images/react-ecosystem/12.jpg)

要是感兴趣，可以看看我用以上技术构建的[示例应用](https://react-example-filmdb.herokuapp.com/)。源码在[Github](https://github.com/tomaash/react-example-filmdb)上。

感谢能坚持阅读到这里 :)

> 本文译自2015年年中的[《Navigating the React.JS Ecosystem》](http://www.toptal.com/react/navigating-the-react-ecosystem) - [Tomas Holas](http://www.toptal.com/resume/tomas-holas)，对原文有理解性改动，水平有限，欢迎提出意见。:)
