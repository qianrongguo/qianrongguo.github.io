永远不会更改位置的`<Router>`。

当用户实际上没有四处点击时，这在服务器端渲染方案中很有用，因此位置永远不会发生实际变化。因此，名称为：静态。当您只需要插入一个位置并在渲染输出中进行断言时，它在简单测试中也很有用。

这是一个示例节点服务器，它为`<Redirect>`发送302状态代码，并为其他请求发送常规HTML：
```jsx harmony
import http from "http";
import React from "react";
import ReactDOMServer from "react-dom/server";
import { StaticRouter } from "react-router";

http
  .createServer((req, res) => {
    // This context object contains the results of the render
    const context = {};

    const html = ReactDOMServer.renderToString(
      <StaticRouter location={req.url} context={context}>
        <App />
      </StaticRouter>
    );

    // context.url will contain the URL to redirect to if a <Redirect> was used
    if (context.url) {
      res.writeHead(302, {
        Location: context.url
      });
      res.end();
    } else {
      res.write(html);
      res.end();
    }
  })
  .listen(3000);
```

### basename: string
所有位置的基本URL。格式正确的基本名称应以斜杠开头，但不能以斜杠结尾。
```jsx harmony
<StaticRouter basename="/calendar">
  <Link to="/today"/> // renders <a href="/calendar/today">
</StaticRouter>
```

### location: string
服务器收到的URL，可能是节点服务器上的req.url。
```jsx harmony
<StaticRouter location={req.url}>
  <App />
</StaticRouter>

```

### location: object
类似的位置对象{ pathname, search, hash, state }
```jsx harmony
<StaticRouter location={{ pathname: "/bubblegum" }}>
  <App />
</StaticRouter>
```

### context: object
一个普通的JavaScript对象。在渲染期间，组件可以向对象添加属性以存储有关渲染的信息。

当`<Route>`匹配时，它将把上下文对象传递给它作为staticContext属性呈现的组件。请查看服务器渲染指南，以获取有关如何自行执行此操作的更多信息。
```jsx harmony
const context = {}
<StaticRouter context={context}>
  <App />
</StaticRouter>
```

当`<Route>`匹配时，它将把上下文对象传递给它作为staticContext属性呈现的组件。请查看服务器渲染指南，以获取有关如何自行执行此操作的更多信息。

渲染后，这些属性可用于配置服务器的响应。
```jsx harmony
if (context.status === "404") {
  // ...
}
```
当`<Route>`匹配时，它将把上下文对象传递给它作为staticContext属性呈现的组件。请查看服务器渲染指南，以获取有关如何自行执行此操作的更多信息。


### children: node
要渲染的子元素。
注意：在React <16上，您必须使用单个子元素，因为render方法不能返回多个元素。如果需要多个元素，则可以尝试将它们包装在额外的`<div>`中。

