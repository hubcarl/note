## commonjs

CommonJS的一个模块就是一个脚本文件。require命令第一次加载该脚本时就会执行整个脚本，然后在内存中生成一个对象

- 对于基本数据类型，属于复制。即会被模块缓存。同时，在另一个模块可以对该模块输出的变量重新赋值。
- 对于复杂数据类型，属于浅拷贝。由于两个模块引用的对象指向同一个内存空间，因此对该模块的值做修改时会影响另一个模块。
- 当使用require命令加载某个模块时，就会运行整个模块的代码。
- 当使用require命令加载同一个模块时，不会再执行该模块，而是取到缓存之中的值。也就是说，commonjs模块无论加载多少次，都只会在第一次加载时运行一次，以后再加载，就返回第一次运行的结果，除非手动清除系统缓存。
- 循环加载时，属于加载时执行。即脚本代码在require的时候，就会全部执行。一旦出现某个模块被"循环加载"，就只输出已经执行的部分，还未执行的部分不会输出。

## ES6模块

- es6模块中的值属于【动态只读引用】。
- 对于只读来说，即不允许修改引入变量的值，import的变量是只读的，不论是基本数据类型还是复杂数据类型。当模块遇到import命令时，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。
- 对于动态来说，原始值发生变化，import加载的值也会发生变化。不论是基本数据类型还是复杂数据类型。
- 循环加载时，ES6模块是动态引用。只要两个模块之间存在某个引用，代码就能够执行

## AMD

AMD是”Asynchronous Module Definition”的缩写，即”异步模块定义”。它采用异步方式加载模块，模块的加载不影响它后面语句的运行。 这里异步指的是不堵塞浏览器其他任务（dom构建，css渲染等），而加载内部是同步的（加载完模块后立即执行回调）。

>代表：requireJS

AMD也采用require命令加载模块，但是不同于CommonJS，它要求两个参数：

- 模块调用

```js
require([module], callback);
```

- 模块编写

```js
define(id?, dependencies?, factory);
```

```js
// AMD
define(['a', 'b'], function(a, b) {
  a.doSomething();
  b.doSomething();
})
```

## CMD

CMD推崇依赖就近，延迟执行

>代表：seajs

```js
// CMD
define(function(require, exports, module) {
  var a = require('./a');
  a.doSomething();
  var b = require('./b');
  b.doSomething();
})

```


## UMD

由于CommonJS是服务器端的规范，跟AMD、CMD两个标准实际不冲突。

```js
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        // AMD
        define(['jquery', 'underscore'], factory);
    } else if (typeof exports === 'object') {
        // Node, CommonJS之类的
        module.exports = factory(require('jquery'), require('underscore'));
    } else {
        // 浏览器全局变量(root 即 window)
        root.returnExports = factory(root.jQuery, root._);
    }
}(this, function ($, _) {
    return {
      ...
    }
}));
```

