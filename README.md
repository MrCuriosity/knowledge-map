## Javascript Foundation
  - prototype & \_\_proto\_\_
  - base data type(latest ECMAScript defined **8** types)
    - Boolean
    - Null
    - Undefined
    - Number
      - [浮点数陷阱及解法](https://github.com/camsong/blog/issues/9)
    - BigInt
    - String
    - Symbol
  - Object
    - structure:[HashMap](https://plushunter.github.io/2017/07/25/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95%EF%BC%8811%EF%BC%89%EF%BC%9A%E5%93%88%E5%B8%8C%E8%A1%A8/)
  - Function
  - iterator
    - [阮一峰es6入门](http://es6.ruanyifeng.com/#docs/iterator)
  - generator
  - Map
  - Set
  - Module

## CSS
  - Negative margin
    - [cnblogs一篇很不错的文章](https://www.cnblogs.com/LiveWithIt/p/6024864.html#commentform)
  - BFC
  - Holy grail layout

---
## React
  - Diff strategy
  - Virtual DOM
  - Fiber
    - [完全理解React Fiber - ayqy](http://www.ayqy.net/blog/dive-into-react-fiber/)
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
    - [ayqy博客 - 一文详解DNS](https://mp.weixin.qq.com/s/0YKV9E4rd77Wc7U4XJmWEQ)
  - Cache
    - 强缓存(Expires/Cache-Control)
    - 协商缓存(Etag/If-None-Match/Last-Modified/If-Modified-Since)
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
    - [一篇infoQ的翻译文章](https://www.infoq.cn/article/how-to-encrypt-the-user-password-correctly)

## Asynchronse
  - Promise
    - [一步一步实现一个Promise](https://juejin.im/post/5c6ad98e6fb9a049d51a0f5e)

## Performance
  - [Google Developer Doc](https://developers.google.com/web/fundamentals/performance/why-performance-matters)

---
## Infrastructure
  - webpack
    - volumn optimization
    - speed optimization
    - dynamically importing
  - module
    - ES6 module(reference importing)
    - CommonJS(value importing)
    - AMD
    - CMD
