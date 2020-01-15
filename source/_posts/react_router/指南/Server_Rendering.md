##  服务器渲染
    
由于服务器都是无状态的，因此在服务器上的渲染有点不同。基本思想是将应用程序包装在无状态的`<StaticRouter>`中，而不是在`<BrowserRouter>`中。我们从服务器传入请求的url，以便路由可以匹配，然后我们将讨论上下文支持。

```js
    // 客户端
    <BrowserRouter>
      <App/>
    </BrowserRouter>
    
    // 服务器端 (not the complete story)
    <StaticRouter
      location={req.url}
      context={context}
    >
      <App/>
    </StaticRouter>
```
当您在客户端上呈现`<Redirect>`时，浏览器历史记录会更改状态，并且我们会获得新屏幕。在静态服务器环境中，我们无法更改应用程序状态。相反，我们使用上下文道具来找出渲染的结果。如果找到context.url，则表明该应用已重定向。这使我们能够从服务器发送适当的重定向。
```js
const context = {};
const markup = ReactDOMServer.renderToString(
  <StaticRouter location={req.url} context={context}>
    <App />
  </StaticRouter>
);

if (context.url) {
  // 某处  `<Redirect>` 是被重定向的
  redirect(301, context.url);
} else {
  // 我们很好，发送响应回复
}
```

### 添加特定于应用程序的上下文信息

路由器仅添加context.url。但是您可能希望将某些重定向重定向为301，将其他重定向重定向为302。或者，如果呈现了UI的某些特定分支，则可能要发送404响应，如果未授权，则要发送401。上下文道具是您的，因此您可以对其进行突变。这是区分301和302重定向的一种方法：
```js
function RedirectWithStatus({ from, to, status }) {
  return (
    <Route
      render={({ staticContext }) => {
        // 客户端上没有“ staticContext”，因此
        // 我们需要在这里提防
        if (staticContext) staticContext.status = status;
        return <Redirect from={from} to={to} />;
      }}
    />
  );
}

// 应用中的某处
function App() {
  return (
    <Switch>
      {/* 其他route */}
      <RedirectWithStatus status={301} from="/users" to="/profiles" />
      <RedirectWithStatus
        status={302}
        from="/courses"
        to="/dashboard"
      />
    </Switch>
  );
}

// 在服务器上
const context = {};

const markup = ReactDOMServer.renderToString(
  <StaticRouter context={context}>
    <App />
  </StaticRouter>
);

if (context.url) {
  // 可以使用 `context.status` 因
  // 我们在 RedirectWithStatus 添加了属性
  redirect(context.status, context.url);
}
```

### 404, 401, or any other status

我们可以做与上述相同的事情。创建一个添加一些上下文的组件，并将其呈现在应用程序中的任何位置以获取不同的状态代码。
```js
function Status({ code, children }) {
  return (
    <Route
      render={({ staticContext }) => {
        if (staticContext) staticContext.status = code;
        return children;
      }}
    />
  );
}
```

现在，您可以在要将代码添加到staticContext的应用程序中的任何位置呈现状态。

```js
function NotFound() {
  return (
    <Status code={404}>
      <div>
        <h1>Sorry, can’t find that.</h1>
      </div>
    </Status>
  );
}

function App() {
  return (
    <Switch>
      <Route path="/about" component={About} />
      <Route path="/dashboard" component={Dashboard} />
      <Route component={NotFound} />
    </Switch>
  );
}
```

### Putting it all together

这不是一个真正的应用程序，但是它显示了将所有内容组合在一起所需的所有常规内容。

```js
import http from "http";
import React from "react";
import ReactDOMServer from "react-dom/server";
import { StaticRouter } from "react-router-dom";

import App from "./App.js";

http
  .createServer((req, res) => {
    const context = {};

    const html = ReactDOMServer.renderToString(
      <StaticRouter location={req.url} context={context}>
        <App />
      </StaticRouter>
    );

    if (context.url) {
      res.writeHead(301, {
        Location: context.url
      });
      res.end();
    } else {
      res.write(`
      <!doctype html>
      <div id="app">${html}</div>
    `);
      res.end();
    }
  })
  .listen(3000);
```
客户端
```js
import ReactDOM from "react-dom";
import { BrowserRouter } from "react-router-dom";

import App from "./App.js";

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById("app")
);
```
### 资料载入
有许多种不同的方法，而且还没有明确的最佳实践，因此我们力求与任何一种方法融为一体，而不是规定或倾向于任何一种方法。我们相信路由器可以放入您的应用程序约束之内。

主要限制是您要在渲染之前加载数据。React Router导出其内部使用的matchPath静态函数以将位置匹配到路由。您可以在服务器上使用此功能来帮助确定呈现之前的数据依赖关系。

这种方法的要旨是依赖于静态路由配置，该配置既可以呈现您的路由，也可以在呈现之前进行匹配以确定数据依赖性。

```js
const routes = [
  {
    path: "/",
    component: Root,
    loadData: () => getSomeData()
  }
  // etc.
];
```
然后使用此配置在应用中呈现您的路线：
```js
import { routes } from "./routes.js";

function App() {
  return (
    <Switch>
      {routes.map(route => (
        <Route {...route} />
      ))}
    </Switch>
  );
}
```
然后，在服务器上您将看到以下内容：
```js
import { matchPath } from "react-router-dom";

// inside a request
const promises = [];
// use `some` to imitate `<Switch>` behavior of selecting only
// the first to match
routes.some(route => {
  // use `matchPath` here
  const match = matchPath(req.path, route);
  if (match) promises.push(route.loadData(match));
  return match;
});

Promise.all(promises).then(data => {
  // do something w/ the data so the client
  // can access it then render the app
});
```
最后，客户将需要提取数据。同样，我们不为您的应用程序规定数据加载模式，但这是您需要实现的接触点。

您可能对我们的React Router Config软件包感兴趣，以通过静态路由配置协助数据加载和服务器渲染。




