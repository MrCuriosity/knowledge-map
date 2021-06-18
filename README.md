## Javascript Foundation

- this 指向，函数中通常指向他的调用者
  - 文件顶层
    - node 环境指向`module.exports`
    - 浏览器环境指向 window
    - 严格模式指向 undefined
  - 构造函数，指向其实例
  - 箭头函数，箭头函数自己没有 this，他的 this 继承自上一级作用域
  - 对象方法，指向调用它的那个对象，比如 a.f()指向 a，a.b.f()指向 a.b（这里 a.b 很明显是一个引用类型）
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
    - 替代有些魔法字符，比如跟业务无关的 tabs`['orders', 'credits']`
    - 避免复杂对象的属性名覆盖
    - 保护私有方法名
- Object

  - structure: [HashMap](https://plushunter.github.io/2017/07/25/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95%EF%BC%8811%EF%BC%89%EF%BC%9A%E5%93%88%E5%B8%8C%E8%A1%A8/)
  - Object.DefineProperty，精确地添加或修改对象的属性，分为
    - 数据描述符
      - `value`，该属性的值，可以是任意的 js 值
      - `writable`，`value`是否可用过赋值运算符`=`改变
        ```javascript
        Object.defineProperty(obj, "key", {
          enumerable: false,
          configurable: false,
          writable: false,
          value: "static",
        });
        ```
    - 存取描述符
      - `get`，取值函数，返回值为属性的值，没有时为 undefined
      - `set`，赋值函数，接收一个参数为赋值的值
        ```javascript
        var o = {};
        var bValue = 38;
        Object.defineProperty(o, "b", {
          get() {
            return bValue;
          },
          set(newValue) {
            bValue = newValue;
          },
          enumerable: true,
          configurable: true,
        });
        ```
    - 可选
      - `enumerable`，是否可枚举(Object.keys)
      - `configurable`，其他操作符属性是否可改变

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
  - 但 WeakMap 与 WeakSet 不同的地方是，其 **值** 本身是对那块内存建立了引用的，不会因为外界的所有引用消失而消失
    ```javascript
    var member1 = { name: "wang" };
    var member1Profile = { job: "FEE" };
    var wm = new WeakMap().set(member1, member1Profile);
    wm.get(member1);
    {
      job: "FEE";
    }
    member1Profile = null;
    // null;
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

      可见 es6 的模块是动态的模块(文件)单例，实时反映了其内部的变化，一个更生动的例子

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

## CSS

- Negative margin
  - [cnblogs 一篇很不错的文章](https://www.cnblogs.com/LiveWithIt/p/6024864.html#commentform)
- BFC
- Holy grail layout

---

## React

- Diff strategy
  - [[WIP]React diff 策略总结](https://github.com/MrCuriosity/blog/blob/master/draft/2021/6/react-diff.md)
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
  - 单个 hook 是闭包，多个 hooks 是多个闭包依赖组成的单链表，这个单链表的信息（每个链表上的信息节点也是一个对象）存储在每个 fiber 节点上，跟随 component 一起创建一起销毁，参考 [React Hooks 原理](https://github.com/brickspert/blog/issues/26)
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
      - Pragma: no-cache 是 HTTP1.0 标准
      - Expires 也是 HTTP1.0 标准, 表示在将来某个时间失效(必须是格林威治时间)
      - Cache-Control 是 HTTP1.1 标准，`Cache-Control: max-age=360000` etc.
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

- HTTP2 [background](https://http2-explained.haxx.se/zh/part1)
- HTTPS（TBD)

## API

- RESTful
- graphQL

---

## Functional Programming

- 函数式有两个基本的优点
  - 引用透明
  - 结果可预测
    > 引用透明本身就通过依赖注入的方式剥离了函数内部对外部的依赖，结果可预测，使得同样的输入一定得到同样的输出，这对函数在程序中的可替换性（重构）有很重要的意义。
- side effect，剥离副作用有两个方案
  - 常见的依赖注入
  - 函子（functor）[函数式编程中如何处理副作用](http://www.ayqy.net/blog/%e5%87%bd%e6%95%b0%e5%bc%8f%e7%bc%96%e7%a8%8b%e4%b8%ad%e5%a6%82%e4%bd%95%e5%a4%84%e7%90%86%e5%89%af%e4%bd%9c%e7%94%a8%ef%bc%9f/)
- [tail invoking](https://juejin.im/entry/592e8a2d0ce463006b510b34)
- recurring
- currying
- compose
- Redux
  - [实现一个 redux](https://github.com/brickspert/blog/issues/22)

---

## Authorization

- password(salt & hashing)
  - [一篇 infoQ 的翻译文章](https://www.infoq.cn/article/how-to-encrypt-the-user-password-correctly)

## Asynchronse

- Promise
  - [一步一步实现一个 Promise](https://juejin.im/post/5c6ad98e6fb9a049d51a0f5e)

## Performance

- react

  - 如何分析和定位
    - 自带的 profiler
      - [Introducing the React Profiler](https://reactjs.org/blog/2018/09/10/introducing-the-react-profiler.html)
      - 找到一段时间内用时过长的部分
      - 用时过长可能是多次 render(diff)
      - 也可能是单次 render 和 commit 过长
      - 由于 profiler 分析的主要是组件的 diff 和 commit，所以它对性能的分析也是有边界的，比如 js 的昂贵计算就分析不到
    - chrome performance tab
      - 这个主要是分析调用栈的消费以及 FPS 变化
  - 有哪些优化手段
    - 避免过深的 props 嵌套，可以用 redux 等直接与数据中心通信，或者`createContext`(其实 redux store 就是封装了上下文的功能)
    - 对于不稳定的 props 对象，避免粗暴的`{ ...props }`，因为后续的 props 膨胀会导致无意义的 rerender
    - PureComponent 替代 Component，避免无意义的 diff(跑到 render()去, 却又 diff 不出来任何变化，此时，从入口到 render 之间的代码开销全是浪费的)
    - memo 包裹函数组件，同上
    - useMemo 来存储一些昂贵的计算(比如 rerender 每次都会路经的平平无奇的代码)
    -
    ```javascript
    const memoResult = useMemo(() => expensiveComputation(props.stableProp), [
      props.stableProp,
    ]);
    ```
    - useCallback 来缓存一些函数，这个倒不一定是为了缓存`昂贵`的函数，而是因为函数经常会创建，函数当做 prop 传入子组件时，浅比较总是会失败，从而导致接收函数的组件 rerender 的，想起早期大家只写类组件时
    ```javascript
    // ...
    render() {
      return (
        <ThisIsAChildComponent onClick={() => console.log(this.props.userName) } />
      )
    }
    // 当这个render方法跑到的时候，onClick的指向总是变，所以<ThisIsAChildComponent />总是rerender
    // 同样道理
    const Father = (props) => {
      const sonOnClick = () => console.log(props.name);
      return <Son onClick={sonOnClick} />
    }
    // 改一下
    const Father = (props) => {
      const sonOnClick = useCallback(() => console.log(props.name), props.name);
      return <Son onClick={sonOnClick} />
    }
    ```

- [Google Developer Doc](https://developers.google.com/web/fundamentals/performance/why-performance-matters)

---

## Infrastructure

- mono-repo & multi-repo
  - 多个项目逻辑上可以独立拆分，单独发布，但彼此间又存在相对的版本依赖，比如一个大 team 的用户 UI，不同端的 UI 与该 UI 的管理系统，此时就比较适合 mono-repo，经典例子就是 React；与之相对的 multi-repo，其实就是我们平时写的单个的 repository。
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

### unresolved questions

- 星号函数的`iterator.next(param)`注入 param 来控制上一个 yield 的输出，原理是什么
