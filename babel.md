

## .babelrc

```
{
  "presets": [
    "es2015",
    "react",
    "stage-0"
  ],
  "plugins": []
}
```


## ES2015 转码规则

- [babel-preset-es2015](http://babeljs.io/docs/plugins/preset-es2015/)

babel-preset-es2015 就包含了es2015 (es6)新增语法的所有转译插件，比如包含 transform-es2015-arrow-functions（es2015箭头函数转译插件）、transform-es2015-classes(es2015 class类转译插件)等

- [babel-preset-es2016](http://babeljs.io/docs/plugins/preset-es2016/)

[transform-exponentiation-operator](http://babeljs.io/docs/plugins/transform-exponentiation-operator/)

- [babel-preset-es2017](http://babeljs.io/docs/plugins/preset-es2016/)

[syntax-trailing-function-commas](http://babeljs.io/docs/plugins/syntax-trailing-function-commas/)
[transform-async-to-generator](http://babeljs.io/docs/plugins/transform-async-to-generator/)

- [babel-preset-env](http://babeljs.io/docs/plugins/preset-env/)


## 不同阶段语法提案的转码规则（共有4个阶段)

- Stage0 ：任何尚未提交为正式提案的讨论，想法，改变或对已有规范的补充建议都被认为是一个稻草人草案（“strawman” proposal），但只有TC39成员可以提出此阶段的草案。babel也提供了对应的转译器 preset-stage-0

- Stage1 ：此阶段，稻草人草案升级为正式化的提案，并将逐步解决多部门关切的问题，如与其他提案的相互之间会有什么影响，这一草案具体该如何实施等问题。人们需要对这些问题提供具体的解决方案。stage1的提案通常还需要包括API描述，拥有说明性使用示例，并对语义和算法进行讨论，一般来说草案在这一阶段会经历巨大的变化。babel也提供了对应的转译器 preset-stage-1

- Stage2 ：此阶段，草案就有了初始的规范。通过polyfill（打补丁。编写一些代码实现浏览器之前不支持的功能），开发者可以开始使用这一阶段的草案了，一些浏览器引擎也会逐步对这一阶段的规范的提供原生支持，此外通过使用构建工具（类似babel的工具）也可以编译源代码为现有引擎可以执行的代码，这些方法都使得这一阶段的草案可以开始被使用了。babel也提供了对应的转译器 preset-stage-2

- State3 ：此阶段的规范就属于候选推荐规范了，这一阶段之后变化就不会那么大了，要达到这一阶段需要满足以下条件：

    规范的编辑和指定的审阅者必须在最终规范上签字；

    用户也应该对该提议感兴趣；

    提案必须至少被一个浏览器原生支持；

    拥有高效的ployfill，或者被Babel支持；

Stage4 ：此阶段的提案必须有两个独立的通过验收测试的实现，进入第4阶段的提案将包含在 ECMAScript 的下一个修订版中。
babel也提供了对应的转译器 preset-stage-3

## stage

http://babeljs.io/docs/plugins/preset-stage-0/

### preset-stage-0

[transform-do-expressions](http://babeljs.io/docs/plugins/transform-do-expressions/)
[transform-function-bind](http://babeljs.io/docs/plugins/transform-function-bind/)

### preset-stage-1

[transform-export-extensions](http://babeljs.io/docs/plugins/transform-export-extensions/)

### preset-stage-2

[syntax-dynamic-import](http://babeljs.io/docs/plugins/syntax-dynamic-import/)
[transform-class-properties](http://babeljs.io/docs/plugins/transform-class-properties/)

### preset-stage-3

[transform-object-rest-spread](http://babeljs.io/docs/plugins/transform-object-rest-spread/)
[transform-async-generator-functions](http://babeljs.io/docs/plugins/transform-async-generator-functions/)



## babel-polyfill


### babel-plugin-transform-xxx

Babel默认只转换新的JavaScript句法（syntax, 箭头函数， 析构等），而不转换新的API，比如 Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise 等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码。

举例来说，ES6在 Array 对象上新增了Array.from方法。Babel 就不会转码这个方法。如果想让这个方法运行，必须使用babel-polyfill。
babel 提供很多对应的 polyfill 插件： babel-plugin-transform-xxx。

- Object.assige: babel-plugin-transform-object-assign

但 ransform 的引用是 module 级别的，这意味着在多个 module 使用时会带来重复的引用，这在多文件的项目里可能带来灾难。另外，你可能也并不想一个个的去添加自己要用的 plugin，如果能自动引入该多好。



### babel-runtime & babel-plugin-transform-runtime

前面提到问题主要在于方法的引入方式是内联的，直接插入了一行代码, 导致每个模块都pollyfill一次代码, 从而无法优化。鉴于这样的考虑，babel 提供了 babel-plugin-transform-runtime，从一个统一的地方 core-js自动引入对应的方法

yarn add -D babel-plugin-transform-runtime
yarn add babel-runtime

```
//.babelrc
{
  "presets": ["env"],
  "plugins": ["transform-runtime"]
}
```

- 上面两种 polyfill 方案共有的缺陷在于作用域。因此 babel 直接提供了通过改变全局来兼容 es2015 所有方法的 babel-polyfill，安装 babel-polyfill 后你只需要在所有代码的最前面加一句 import 'babel-polyfill' 便可引入它.

- 如果你是在开发一个库或者框架，那么 babel-polyfill 的体积就有点大了，尤其是在你实际使用的只有一个 Object.assign 的情况下。更可怕的是对于一个库来说，改变全局环境是使不得的。谁也不希望使用了你的库，还附带了一家老小的 polyfill 改变了全局对象。这时不污染全局环境的 babel-plugin-transform-runtime 才是最合适的。


## babel-preset-env

通过 babel-preset-env 自动识别代码引入 polyfill . 它可以根据指定目标环境判断需要做哪些编译。而且 babel-preset-env 也支持针对指定目标环境选择需要的 polyfill 了，只需引入 babel-polyfill，并在 babelrc 中声明 useBuiltIns，babel 会将引入的 babel-polyfill 自动替换为所需的 polyfil


## http://polyfill.io


https://zhuanlan.zhihu.com/p/27777995
