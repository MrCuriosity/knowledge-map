## Javascript Foundation

- this
  - 文件顶层
  - 构造函数
  - 箭头函数
  - 对象方法
- prototype & \_\_proto\_\_ [自己写的原型链总结](https://github.com/MrCuriosity/blog/blob/master/draft/2021/2/prototype.md)
- base data type(latest ECMAScript defined **8** types)
  - Boolean
  - Null(ES6 提出了 Null 类型，但由于历史原因 typeof null === 'object')
  - Undefined
  - Number
    - [浮点数陷阱及解法](https://github.com/camsong/blog/issues/9)
  - BigInt
  - String
  - Symbol
- Object
  - structure: [HashMap](https://plushunter.github.io/2017/07/25/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95%EF%BC%8811%EF%BC%89%EF%BC%9A%E5%93%88%E5%B8%8C%E8%A1%A8/)
  - Object.DefineProperty
- Function
- iterator
  - [阮一峰 es6 入门 - 迭代器](http://es6.ruanyifeng.com/#docs/iterator)
- generator
- Map
  - 不同于普通的对象"key-value"对应，Map 结构提供"value-value"对应，即任何值都可以作为键名.
  - 键名按内存地址来比较, 因此`const k1 = ['a']`和`const k2 = ['a']`是两个不同的键, 唯一例外虽然`NaN !== NaN`, 但它在 Map 中被视为同一个键
- WeakMap
  - WeakMap 只能添加引用类型(null 除外)
  - WeakMap 中的引用都是弱引用，作为键名的引用类型如果将来消失(如可能被删除的 DOM 节点)，则 WeakMap 失去对其的引用，这一点于 WeakSet 类似:
    ```javascript
    var member1 = { name: "wang" };
    var wm = new WeakMap().set(member1, "exists");
    wm.get(member1);
    // "exists"
    member1 = null;
    // null
    wm.get(member1);
    // undefined
    ```
  - 但 WeakMap 与 WeakSet 不同的地方是，其值是建立了引用的，不会因为外界的所有引用消失而消失
    ```javascript
    var member1 = { name: "wang" };
    var member1Profile = { job: "FEE" };
    var wm = new WeakMap().set(member1, member1Profile);
    wm.get(member1);
    {
      job: "FEE";
    }
    member1Profile = null;
    null;
    wm.get(member1);
    {
      job: "FEE";
    }
    ```
- Set
  - 集合可以被遍历(Symbol.iterator 接口), 它是有序的
- WeakSet
  - 弱集合只能添加引用类型
  - 且弱集合中的引用都是弱引用，即不作为垃圾回收机制的的参考，因此弱集合的元素随时都可能变化（元素真实的引用全部失去，被 GC 回收)
- Module

  - import
    - es6 module 是静态加载，是编译时就输出好的代码
    - 由于是静态加载，所以可以进行静态分析、优化
    - 模块内部的变量外部无法获取，如要获取，则须通过 export 暴露出来.
    - 一个文件中，同一个模块可以引用多次，但它是单例的
      ```javascript
      import { foo } from "test";
      import { bar } from "test";
      ```
  - import()
    - 类似于 node 的 require 命令，运行时加载
    - 由于运行时加载，所以与静态 import 不同，import()可以存在条件语句当中，它的返回值是一个 promise
      ```javascript
      import(`./locales/${adaptedLocaleCode}.json`).then((messages) => {
        setLocale(messages);
      });
      ```
  - script 标签
    - 默认是 javascript，所以`type="application/javascript"`可以省略
    - \<script\> 默认是同步加载，所以会阻塞 HTML 的渲染，`defer`和`async`属性会开启异步加载
      - `defer`会等页面中正常渲染结束(DOM 结构完全生成，及其他脚本执行完成)才会执行
      - `async`不会阻塞 DOM 的渲染，但是一旦脚本下载完，就会暂停 DOM 渲染执行脚本
      - 所以根据以上两点，如果有多个`defer`脚本，他们会形成顺序加载的效果; 而多个`async`会几乎同时开始下载，但谁先执行完成是无法保证的.
  - CommonJS

    - CommonJS 输出值的拷贝，而 es6 module 输出值的引用.

      ```javascript
      // lib.js
      var counter = 3;
      function incCounter() {
        counter++;
      }
      module.exports = {
        counter: counter,
        incCounter: incCounter,
      };

      // main.js
      var mod = require("./lib");

      console.log(mod.counter); // 3
      mod.incCounter();
      console.log(mod.counter); // 3
      ```

      ```javascript
      export let counter = 3;
      export function incCounter() {
        counter++;
      }

      // main.js
      import { counter, incCounter } from "./lib";
      console.log(counter); // 3
      incCounter();
      console.log(counter); // 4
      ```

      可见 es6 的模块是动态的，实时反映了其内部的变化，一个更生动的例子

      ```javascript
      // m1.js
      export var foo = "bar";
      setTimeout(() => (foo = "baz"), 500);

      // m2.js
      import { foo } from "./m1.js";
      console.log(foo);
      setTimeout(() => console.log(foo), 500);
      ```

    - CommonJS 运行时加载，es6 module 编译时输出接口.
    - require()是同步加载，而 import 是异步加载, 但有一个独立的模块依赖解析阶段
    -

## CSS

- Negative margin
  - [cnblogs 一篇很不错的文章](https://www.cnblogs.com/LiveWithIt/p/6024864.html#commentform)
- BFC
- Holy grail layout

---

## React

- Diff strategy
- Virtual DOM
- diff
  - [React list diff - ayqy](http://www.ayqy.net/blog/react-list-diff/)
- Fiber
  - [完全理解 React Fiber - ayqy](http://www.ayqy.net/blog/dive-into-react-fiber/)
- Component & PureComponent
- StatelessComponent & React.memo
- Lifecycle
- Ref(implement DOM directly)
- stateless component & class component
- Hooks
  - [Official Doc](https://reactjs.org/docs/hooks-intro.html)
- Lists & Keys
  - [Official Doc](https://reactjs.org/docs/lists-and-keys.html)
- Higher Order Component
  - [Official Doc](https://reactjs.org/docs/higher-order-components.html)
- Render Props
  - [Official Doc](https://reactjs.org/docs/render-props.html)
- Portals
  - [Official Doc](https://reactjs.org/docs/portals.html)
- Fragment
  - [Official Doc](https://reactjs.org/docs/fragments.html)
- React.la

---

## HTTP

- Idempotent
  - [en](https://developer.mozilla.org/en-US/docs/Glossary/Idempotent)
  - [zh](https://developer.mozilla.org/zh-CN/docs/Glossary/%E5%B9%82%E7%AD%89)
- three way handshake
- cross domain
  - JSONP
  - CORS
  - proxy(e.g. nginx)
- security
  - XSS
    - [一篇不错的文章](https://www.cxymsg.com/guide/security.html#%E5%A6%82%E4%BD%95%E9%A2%84%E9%98%B2xss)
  - CSRF
- oauth
- DNS
  - [ayqy 博客 - 一文详解 DNS](https://mp.weixin.qq.com/s/0YKV9E4rd77Wc7U4XJmWEQ)
- Cache
  - 静态资源缓存
    - 强缓存(Pragma/Cache-Control/Expires)
      - Expires 是 HTTP1.0 标准, 表示在将来某个时间失效(必须是格林威治时间)
      - Cache-Control 是 HTTP1.1 标准
      - 为了做向下兼容, 三者可能同时出现, 如果同时出现, 优先级为 Pragma -> Cache-Control -> Expires
    - 协商缓存(Etag/If-None-Match/Last-Modified/If-Modified-Since)
      - Etag/If-None-Match(基于资源内容做的 hash)
      - Last-Modified/If-Modified-Since(基于资源修改时间的 hash, 如果资源的内容没有发生实际上的变化, 但修改时间发生了变化, 这个响应头也会变化, 导致浪费资源获取一样的内容)
      - 二者同时出现时, Etag 优先级高于 If-Modified-Since
    - 无论是强缓存和协商缓存, 在 chrome 中命中时都会有(from disk cache)和(from memory cache)的形式, 目前标准不明, 可能是浏览器会计算其大小, 来决定是否放在内存中, 从而达到更快的调取速度.
  - 数据缓存
    - cookie(4k, 保持服务器会话状态, 可被拦截, 无法跨域调用)
    - sesstionStorage
    - localStorage(生命周期与 cookie 不同, 稍大一点, 5M 左右, 可以做一些数据的本地保持, 故而生命周期理论上是无限的, 受该域下 JS 控制, 可以用于表单数据保持, 登陆状态保持等)

## API

- RESTful
- graphQL

## HTTP2

- [background](https://http2-explained.haxx.se/zh/part1)

---

## Functional Programming

- [side effect](./source/functional_programming/side_effect.md)
- pure function
- [tail invoking](https://juejin.im/entry/592e8a2d0ce463006b510b34)
- recurring
- currying
- compose

---

## Authorization

- password(salt & hashing)
  - [一篇 infoQ 的翻译文章](https://www.infoq.cn/article/how-to-encrypt-the-user-password-correctly)

## Asynchronse

- Promise
  - [一步一步实现一个 Promise](https://juejin.im/post/5c6ad98e6fb9a049d51a0f5e)

## Performance

- [Google Developer Doc](https://developers.google.com/web/fundamentals/performance/why-performance-matters)

---

## Infrastructure

- mono-repo & multi-repo
  - 多个项目逻辑上可以独立拆分，单独发布，但彼此间又存在相对的版本依赖，比如一个大 team 的用户 UI，不同端的 UI 该 UI 的管理系统，此时就比较适合 mono-repo，经典例子就是 React；与之相对的 multi-repo，其实就是我们平时写的单个的 repository。
  - dependency control，这或许是开发中最大的好处，公用的依赖可以放在最顶层，子项目要单独使用的版本可以写在子项目的 package.json.
  - 一致性，包括项目构建、代码风格、lint 规则、覆盖率要求等
  - 多个子仓库可能依赖某些 shared 仓库，比如一个公司中有统一 design guideline 的 components，自定义 hooks，都可以成为单独的子仓库而被别的业务仓库 import，在提升了一致性和复用性的同时还简化了子仓库间的依赖管理。
  - lerna
    - 版本控制分为锁定模式和独立模式，由项目根目录的`lerna.json`文件中的 version 字段决定
      - 锁定模式，version 是一个具体的版本号，也同时同步了所有子项目的版本号
      - 独立模式，`version: "independent"`, 每个子项目都需要在发布时确定其版本号
- webpack
  - volumn optimization
  - speed optimization
  - dynamically importing
- event loop
  - macro task: setTimeout, setInterval, setImmediate, requestAnimationFrame, I/O, UI rendering
  - micro task: process.nextTick, Promises, queueMicrotask, MutationObserver
- 设计模式
  - 面向对象设计原则，"SOLID"
    - 单一原则（Single Responsibility Principle），高内聚低耦合，一个类只做一个事情，耦合和内聚的界限参考实际情况和个人的理解
    - 开闭原则（Open-Close Principle），对扩展开放，对修改关闭；这个原则在软件的生命周期中是用的很多的，包括多人协作的项目 code review 中也是一个很重要的原则，可以避免对旧功能的修改而导致的不稳定和重复 QA；
    - 里氏替换原则（Liskov Substitution Principle），继承和多态的应用，简单的说就是父类出现的任何地方都可以被子类替换，且程序原有的行为不变。
      - 继承，重载（overload），多态（polymorphism）
    - 接口隔离原则（Interface Segregation Principle），接口的调用者不应该被迫依赖于他用不到的一些方法，比如 A 接口实现了 abcde 方法，B 只需要 abc 方法，则最小依赖其实是 abc，de 可以通过扩展得到。看起来跟单一职责有点类似，但 SRP 是从接口、类的设计角度来看的，ISP 则是通过接口的使用者对接口的依赖间接来看的。
    - 依赖翻转原则（Dependence Inversion Principle），A 类的 a 方法具有对 b 方法的依赖，如此编程，则高层依赖底层，底层一改变则高层实现全要变，这样耦合度是很高的，程序会越来越难以维护。依赖反转则可以使高层依赖底层，这里并不是完全指的是字面意义上的反过来设计程序，比如一个基础 dropdown -> 多选 dropdown -> 带搜索功能的 dropdown，包括了 input 组件和 tag 组件，这里并不是从成品开始拆成最细粒度的组件，而是实现相同的接口，那么底层和高层都无所谓了，只要实现了约定的接口，无论基础的 dropdown 还是高层的 input 和 tag 都变得可拆卸，比如 typescript 这样的静态类型语言配合泛型就能很好的解决这个问题。个人理解，声明式编程天然的解决了依赖反转的问题。
