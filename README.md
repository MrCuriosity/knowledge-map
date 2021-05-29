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

## CSS

- Negative margin
  - [cnblogs 一篇很不错的文章](https://www.cnblogs.com/LiveWithIt/p/6024864.html#commentform)
- BFC
- Holy grail layout

---

## React

- Diff strategy
- Virtual DOM
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
  - lerna
- webpack
  - volumn optimization
  - speed optimization
  - dynamically importing
- module
  - ES6 module(reference importing)
  - CommonJS(value importing)
  - AMD
  - CMD
