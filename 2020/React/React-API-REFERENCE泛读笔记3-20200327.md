# React API REFERENCE

[React 官网](https://zh-hans.reactjs.org/)

## ReactDOM

- SSR : 服务端渲染（Server Side Render）

  - SSR 可以解决的问题
    - SEO
    - 加速首屏加载

- 服务端渲染字符串：
  - renderToString() --> string 客户端渲染
  - renderToStaticMarkup() --> string 客户端渲染
- 客户端渲染：
  - render() --> HTML 结构
  - hydrate()

## ReactDOMServer

- 将组件渲染成静态标记，通常被使用在 Node 服务端，有 4 个方法

  - renderToString()，服务端渲染字符串

    - 将 React 元素渲染为初始的 HTML。React 将返回一个 HTML 字符串。可以使用该方法在服务端生成 HTML，在首次请求时将标记下发，以加快页面加载速度，并允许搜索引擎爬取页面达到 SEO 优化的目的

  - renderToStaticMarkup()，服务端渲染字符串

    - 希望把 React 当作静态页面生成器的时候，此方法非常有用，还能去除额外的属性来节省一些字节

  - renderToNodeStream()

    - 将 React 元素渲染为初始的 HTML。返回一个可输出的 HTML 字符串的可读流，该可读流输出的 HTML 完全等同于`ReactDOMServer.renderToString()`返回的 HTML，可以使用该方法在服务器上生成 HTML，在首次请求时将标记下发，以加快页面加载速度，并允许搜索引擎爬取页面达到 SEO 优化的目的

    - **注意：** 该 API 仅允许在服务端使用。不允许在浏览器使用，返回的是一个 utf-8 编码的字节流

- renderToStaticNodeStream()

  ```javascript
  ReactDOMServer.renderToStaticNodeStream(element);
  ```

  - 该 API 可读流输出的 HTML，完全等同于`ReactDOMServer.renderToStaticMarkup()`返回的 HTML

  - **注意：** 该 API 仅允许在服务端使用。不允许在浏览器使用，返回的是一个 utf-8 编码的字节流
